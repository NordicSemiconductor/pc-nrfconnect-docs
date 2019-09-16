---
---

## Adding items

The navigation menu is rendered by the `NavMenu` component. The component
receives `menuItems` as a property, so adding your own menu items can be done
like this:

```
export function decorateNavMenu(NavMenu) {
    return props => (
        <NavMenu
            {...props}
            menuItems={[
                { id: 0, text: 'Search', iconClass: 'icon-search' },
                { id: 1, text: 'Settings', iconClass: 'icon-wrench' },
            ]}
        />
    );
}
```

The `decorateNavMenu` function receives the `NavMenu` component, and must return
a new React component. We return a
[functional component](https://facebook.github.io/react/docs/components-and-props.html#functional-and-class-components)
here, but a React class component could also have been used.

Our functional component receives the `props` that are passed to `NavMenu`. See
the
[NavMenu source code](https://github.com/NordicSemiconductor/pc-nrfconnect-core/blob/master/lib/components/NavMenu.jsx)
to see which props the component expects. We override the `menuItems` prop to
pass in our own menu items, but keep the rest of the props by using the
[spread syntax](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator).

## Use selected menu item

We have now added some items, but so far nothing will happen in the UI when
selecting them. By default, when clicking a menu item, a
`NAV_MENU_ITEM_SELECTED` action is dispatched with the selected item ID. This
action is handled by the
[navMenuReducer](https://github.com/NordicSemiconductor/pc-nrfconnect-core/blob/master/lib/windows/app/reducers/navMenuReducer.js),
which sets `state.core.navMenu.selectedItemId`.

Let us say you want to render different content in the `MainView` based on the
selected menu item. Then pass `state.core.navMenu.selectedItemId` to the
`MainView` using `mapMainViewState`:

```
export function mapMainViewState(state, props) {
    return {
        ...props,
        selectedMenuItemId: state.core.navMenu.selectedItemId,
    };
}
```

The `MainView` will now receive a `selectedMenuItemId` prop that you can use to
render conditionally:

```
export function decorateMainView(MainView) {
    return props => (
        <MainView>
            {
                props.selectedMenuItemId === 0 ?
                    <h1>Search</h1> :
                    <h1>Settings</h1>
            }
        </MainView>
    );
}
```

In this example we use `h1` tags, but in practice you would of course create
React components for search and settings, and render those instead.

## Full example

To sum up, here is a full example of how navigation can be implemented in an
app:

```
import React from 'react';

const views = [
    { id: 0, text: 'Search', iconClass: 'icon-search' },
    { id: 1, text: 'Settings', iconClass: 'icon-wrench' },
];

export function decorateNavMenu(NavMenu) {
    return props => (
        <NavMenu
            {...props}
            menuItems={views}
        />
    );
}

export function mapMainViewState(state, props) {
    return {
        ...props,
        selectedMenuItemId: state.core.navMenu.selectedItemId,
    };
}

export function decorateMainView(MainView) {
    return props => {
        const view = views[props.selectedMenuItemId];
        const title = view ? view.text : 'Welcome';
        return (
            <MainView>
                <h1>{ title }</h1>
            </MainView>
        );
    };
}
```
