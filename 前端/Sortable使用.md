Sortable
https://github.com/SortableJS/Sortable

# Store
Saving and restoring of the sort.

```html
<ul>
    <li data-id="1">order</li>
    <li data-id="2">save</li>
    <li data-id="3">restore</li>
</ul>

```
```javascript
Sortable.create(el, {
    group: "localStorage-example",
    store: {
        /**
         * Get the order of elements. Called once during initialization.
         * @param   {Sortable}  sortable
         * @returns {Array}
         */
        get: function (sortable) {
            var order = localStorage.getItem(sortable.options.group.name);
            return order ? order.split('|') : [];
        },

        /**
         * Save the order of elements. Called onEnd (when the item is dropped).
         * @param {Sortable}  sortable
         */
        set: function (sortable) {
            var order = sortable.toArray();
            localStorage.setItem(sortable.options.group.name, order.join('|'));
        }
    }
})
```
