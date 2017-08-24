# 2017/06/22 10:12:55
cefclient 窗口管理

窗口管理涉及到如下的类：

1. ClientHandler 
2. BrowserWindow
3. BrowserWindowStdWin
4. RootWindow
5. RootWindowWin
6. RootWindowManager

下面我对每个类的理解

## ClientHandler
这个类看它继承的类，就可以很清楚的知道它的用途。它用于处理浏览器窗口的各种类型的事件。

    class ClientHandler : public CefClient,
                          public CefContextMenuHandler,
                          public CefDisplayHandler,
                          public CefDownloadHandler,
                          public CefDragHandler,
                          public CefFocusHandler,
                          public CefGeolocationHandler,
                          public CefKeyboardHandler,
                          public CefLifeSpanHandler,
                          public CefLoadHandler,
                          public CefRequestHandler {
                            ...
                          }

## BrowserWindow
这个类代表一个浏览器窗口，它继承 ClientHandler::Delegate，这样就可以处理一些特定的浏览器事件
    
    class BrowserWindow : public ClientHandler::Delegate {
        ...

        CefRefPtr<CefBrowser> browser_;
        CefRefPtr<ClientHandler> client_handler_;
    }

## BrowserWindowWin
同 BrowserWindow，但是 Windows 平台特定。

    class BrowserWindowStdWin : public BrowserWindow {
    }

## RootWindow
RootWindow 代表一个父窗口。注意 cefclient 中的窗口结构，每一个浏览器窗口是作为一个原生 windows 窗口的子窗口的形式存在的。这个 RootWindow 就代表这个原生的 Windows 窗口，它拥有浏览器窗口。

    class RootWindow :
        public base::RefCountedThreadSafe<RootWindow, DeleteOnMainThread> {
        }

## RootWindowWin
Windows 平台特定。注意 RootWindowWin 继承了 BrowserWindow::Delegate，这样它就可以处理 BrowserWindow 提供的一些事件。

    class RootWindowWin : public RootWindow,
                                           public BrowserWindow::Delegate {
        ...

        scoped_ptr<BrowserWindow> browser_window_;
    }

## RootWindowManager                
RootWindowManager 表示一个窗口管理器，管理所有的父窗口。它继承 RootWindow::Delegate，这样就可以处理每个父窗口提供的一些事件。从而对父窗口进行管理。

    class RootWindowManager : public RootWindow::Delegate {
        typedef std::set<scoped_refptr<RootWindow> > RootWindowSet;
        RootWindowSet root_windows_;
    }       

# 窗口创建流程
窗口的创建分为两种情况。首先是，程序启动后，要创建第一个窗口；其次是当我们点击网页中的链接时，有时会创建一个新的窗口，这种窗口我们称之为popup窗口。这两种情况的窗口创建流程是不一样的。

## 创建第一个主窗口
创建第一个窗口从调用 RootWindowManager::CreateRootWindow 开始。

  scoped_refptr<RootWindow> RootWindowManager::CreateRootWindow(
      bool with_controls,
      bool with_osr,
      const CefRect& bounds,
      const std::string& url) {
    CefBrowserSettings settings;
    MainContext::Get()->PopulateBrowserSettings(&settings);

    scoped_refptr<RootWindow> root_window =
        RootWindow::Create(MainContext::Get()->UseViews());
    root_window->Init(this, with_controls, with_osr, bounds, settings,
                      url.empty() ? MainContext::Get()->GetMainURL() : url);

    // Store a reference to the root window on the main thread.
    OnRootWindowCreated(root_window);

    return root_window;
  }

RootWindowManager::CreateRootWindow 会创建一个 RootWindowWin 对象，然后后续工作交给这个 RootWindowWin 对象。

第二步，RootWindowWin::Init，这个函数由上面 RootWindowManager::CreateRootWindow 调用。

  void RootWindowWin::Init(RootWindow::Delegate* delegate,
                           bool with_controls,
                           bool with_osr,
                           const CefRect& bounds,
                           const CefBrowserSettings& settings,
                           const std::string& url) {
    DCHECK(delegate);
    DCHECK(!initialized_);

    delegate_ = delegate;
    with_controls_ = with_controls;
    with_osr_ = with_osr;

    start_rect_.left = bounds.x;
    start_rect_.top = bounds.y;
    start_rect_.right = bounds.x + bounds.width;
    start_rect_.bottom = bounds.y + bounds.height;

    CreateBrowserWindow(url);

    initialized_ = true;

    // Create the native root window on the main thread.
    if (CURRENTLY_ON_MAIN_THREAD()) {
      CreateRootWindow(settings);
    } else {
      MAIN_POST_CLOSURE(
          base::Bind(&RootWindowWin::CreateRootWindow, this, settings));
    }
  }

它会创建一个 BrowserWindowStdWin 对象，然后调用 CreateRootWindow 完成主窗口的创建，也就是 windows 原生窗口的创建。

第三步，RootWindowWin::CreateRootWindow，这个函数主要做两件事，首先就是创建主窗口，然后调用 BrowserWindowStdWin::CreateBrowser 完成最终浏览器窗口的创建。

主窗口的创建流程就是这样。

## 创建 popup 窗口
### 在 popup 窗口创建之前对 popup 窗口进行设置
创建一个 popup 窗口的起点在 ClientHandler::OnBeforePopup，该函数是 ClientHandler 实现的父类 CefLifeSpanHandler 提供的虚函数 OnBeforePopup，下面是关于这个函数的简单介绍：

  Called on the IO thread before a new popup browser is created

    bool ClientHandler::OnBeforePopup(
        CefRefPtr<CefBrowser> browser,
        CefRefPtr<CefFrame> frame,
        const CefString& target_url,
        const CefString& target_frame_name,
        CefLifeSpanHandler::WindowOpenDisposition target_disposition,
        bool user_gesture,
        const CefPopupFeatures& popupFeatures,
        CefWindowInfo& windowInfo,
        CefRefPtr<CefClient>& client,
        CefBrowserSettings& settings,
        bool* no_javascript_access) {
      CEF_REQUIRE_IO_THREAD();

      // Return true to cancel the popup window.
      return !CreatePopupWindow(browser, false, popupFeatures, windowInfo, client,
                                settings);
    }

如上 ClientHandler::OnBeforePopup 会直接调用 ClientHandler::CreatePopupWindow，

    bool ClientHandler::CreatePopupWindow(
        CefRefPtr<CefBrowser> browser,
        bool is_devtools,
        const CefPopupFeatures& popupFeatures,
        CefWindowInfo& windowInfo,
        CefRefPtr<CefClient>& client,
        CefBrowserSettings& settings) {
      // Note: This method will be called on multiple threads.

      // The popup browser will be parented to a new native window.
      // Don't show URL bar and navigation buttons on DevTools windows.
      MainContext::Get()->GetRootWindowManager()->CreateRootWindowAsPopup(
          !is_devtools, is_osr(), popupFeatures, windowInfo, client, settings);

      return true;
    }

ClientHandler::CreatePopupWindow 又直接去调用 RootWindowManager::CreateRootWindowAsPopup，

    scoped_refptr<RootWindow> RootWindowManager::CreateRootWindowAsPopup(
        bool with_controls,
        bool with_osr,
        const CefPopupFeatures& popupFeatures,
        CefWindowInfo& windowInfo,
        CefRefPtr<CefClient>& client,
        CefBrowserSettings& settings) {
      MainContext::Get()->PopulateBrowserSettings(&settings);

      scoped_refptr<RootWindow> root_window =
          RootWindow::Create(MainContext::Get()->UseViews());
      root_window->InitAsPopup(this, with_controls, with_osr,
                               popupFeatures, windowInfo, client, settings);

      // Store a reference to the root window on the main thread.
      OnRootWindowCreated(root_window);

      return root_window;
    }

RootWindowManager::CreateRootWindowAsPopup 的逻辑和 RootWindowManager::CreateRootWindow 基本一致，差别就在于 RootWindowManager::CreateRootWindowAsPopup 在创建了一个 RootWindowWin 对象后，会调用 RootWindowWin::InitAsPopup 函数

RootWindowWin::InitAsPopup 同样会创建一个  BrowserWindowWin 对象，但它不会调用 RootWindowWin::CreateRootWindow，注意这里的区别。

    void RootWindowWin::InitAsPopup(RootWindow::Delegate* delegate,
                                    bool with_controls,
                                    bool with_osr,
                                    const CefPopupFeatures& popupFeatures,
                                    CefWindowInfo& windowInfo,
                                    CefRefPtr<CefClient>& client,
                                    CefBrowserSettings& settings) {
      DCHECK(delegate);
      DCHECK(!initialized_);

      delegate_ = delegate;
      with_controls_ = with_controls;
      with_osr_ = with_osr;
      is_popup_ = true;

      if (popupFeatures.xSet)
        start_rect_.left = popupFeatures.x;
      if (popupFeatures.ySet)
        start_rect_.top = popupFeatures.y;
      if (popupFeatures.widthSet)
        start_rect_.right = start_rect_.left + popupFeatures.width;
      if (popupFeatures.heightSet)
        start_rect_.bottom = start_rect_.top + popupFeatures.height;

      CreateBrowserWindow(std::string());

      initialized_ = true;

      // The new popup is initially parented to a temporary window. The native root
      // window will be created after the browser is created and the popup window
      // will be re-parented to it at that time.
      browser_window_->GetPopupConfig(TempWindow::GetWindowHandle(),
                                      windowInfo, client, settings);
    }

然后是 BrowserWindowStdWin::GetPopupConfig

    void BrowserWindowStdWin::GetPopupConfig(CefWindowHandle temp_handle,
                                             CefWindowInfo& windowInfo,
                                             CefRefPtr<CefClient>& client,
                                             CefBrowserSettings& settings) {
      // Note: This method may be called on any thread.
      // The window will be properly sized after the browser is created.
      windowInfo.SetAsChild(temp_handle, RECT());
      client = client_handler_;
    }

通过处理 OnBeforePopup 事件，我们其实能做的就是在这个 popup 窗口创建之前，对这个窗口的一些相关属性进行修改，比如windowInfo、 client、settings。这其实就是上面整个流程中我们做的事情。在 RootWindowManager::CreateRootWindowAsPopup 中，我们设置了 settings，在 BrowserWindowStdWin::GetPopupConfig 我们有设置了 windowInfo 和 client。

注意，这里非常有意思的一点，我们将这个要创建的 popup 窗口设置为一个临时窗口的子窗口，而这个 popup 窗口的真正父窗口，在这个流程中并没有创建。

## 创建 popup 窗口的父窗口
在上面的流程中，并没有创建 popup 的父窗口，因为那时，popup 窗口也没有创建。那父窗口在什么时间创建呢？

父窗口的创建从 ClientHandler::OnAfterCreated 开始，也就是在 popup 窗口创建之后。

    void ClientHandler::OnAfterCreated(CefRefPtr<CefBrowser> browser) {
      CEF_REQUIRE_UI_THREAD();

      browser_count_++;

      ...

      NotifyBrowserCreated(browser);
    }

    void ClientHandler::NotifyBrowserCreated(CefRefPtr<CefBrowser> browser) {

    ...

      if (delegate_)
        delegate_->OnBrowserCreated(browser);
    }

    void BrowserWindow::OnBrowserCreated(CefRefPtr<CefBrowser> browser) {
      REQUIRE_MAIN_THREAD();
      DCHECK(!browser_);
      browser_ = browser;

      delegate_->OnBrowserCreated(browser);
    }


    void RootWindowWin::OnBrowserCreated(CefRefPtr<CefBrowser> browser) {
      REQUIRE_MAIN_THREAD();
      if (is_popup_) {
        // For popup browsers create the root window once the browser has been
        // created.
        CreateRootWindow(CefBrowserSettings());
      } else {
        // Make sure the browser is sized correctly.
        OnSize(false);
      }
    }

如上，OnAfterCreated 事件一一路向上传递，最后交给 RootWindowWin::OnBrowserCreated 处理，RootWindowWin::OnBrowserCreated 在调用 CreateRootWindow 创建窗口。


处理，RootWindowWin::CreateRootWindow 在创建完父窗口后，会调用 BrowserWindowStdWin::ShowPopup，将浏览器窗口的父窗口设置为它创建的父窗口。

    void BrowserWindowStdWin::ShowPopup(ClientWindowHandle parent_handle,
                                        int x, int y, size_t width, size_t height) {
      REQUIRE_MAIN_THREAD();

      HWND hwnd = GetWindowHandle();
      if (hwnd) {
        SetParent(hwnd, parent_handle);
        SetWindowPos(hwnd, NULL, x, y,
                     static_cast<int>(width), static_cast<int>(height),
                     SWP_NOZORDER);
        ShowWindow(hwnd, SW_SHOW);
      }
    }