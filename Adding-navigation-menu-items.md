## Adding items

The navigation menu is rendered by the `NavMenu` component. The component receives `menuItems` as a property, so adding your own menu items can be done like this:

```
decorateNavMenu: NavMenu => (
    props => (
        <NavMenu
            {...props}
            menuItems={[
                { id: 'SEARCH', text: 'Search', iconClass: 'icon-search' },
                { id: 'SETTINGS', text: 'Settings', iconClass: 'icon-wrench' },
            ]}
        />
    )
),
```

The `decorateNavMenu` function receives the `NavMenu` component, and must return a new React component. We return a [functional component](https://facebook.github.io/react/docs/components-and-props.html#functional-and-class-components) here, but a React class component could also have been used.

Our functional component receives the `props` that are passed to `NavMenu`. See the [NavMenu source code](https://github.com/NordicSemiconductor/pc-nrfconnect-core/blob/master/lib/components/NavMenu.jsx) to see which props the component expects. We override the `menuItems` prop to pass in our own menu items, but keep the rest of the props by using the [spread syntax](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator).

## Use selected menu item

TODO