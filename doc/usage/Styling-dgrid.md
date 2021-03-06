# Styling dgrid

dgrid components are designed to be highly CSS-driven for optimal performance and
organization, so visual styling should be controlled through CSS.

## Styling Grid Columns

The Grid module creates classes based on the field names and column IDs of the columns
defined, following the conventions `field-<field-name>` and `dgrid-column-<column-id>`.
You can also explicitly define one or more classes via the `className` property,
in which case it is applied in addition to `field-<field-name>`.

For example, you could define a grid and style it like so:

```html
<style>
    .field-age {
        width: 80px;
    }
    .field-first {
        font-weight: bold;
    }
</style>
<script>
    require([ 'dgrid/Grid' ], function (Grid) {
        var grid = new Grid({
            columns: {
                age: 'Age',
                first: 'First Name',
                // ...
            }
        }, 'grid');
        grid.renderArray(someData);
    });
</script>
```

## Styling Grid Rows

While the column definition provides mechanisms for adding classes to individual
columns, there is no similar construct at the row level.  However, it is possible
to add classes to rows by aspecting `renderRow`.

However, you can achieve this using `dojo/aspect.after` - when its
`receiveArguments` argument is `false` (the default), it passes the original
arguments object as a second parameter to the advising function, allowing access
to the original object:

```js
aspect.after(grid, 'renderRow', function(row, args) {
  // Add classes to `row` based on `args[0]` here
  return row;
});
```

## Skinning dgrid Components

dgrid automatically loads the necessary structural CSS to work properly using
xstyle's css module.  However, to make the component far more visually attractive,
it is common to also apply one of the the included skins.  There are various
CSS files under the `css/skins` directory which can be used to significantly
alter the look and feel of dgrid components.  The `skin.html` test page provides
a quick demonstration of all included skins.

Many of the classes commonly involved in customizing layout and skin can be discovered
by examining the CSS of existing skins, or by inspecting elements in your browser's
developer tools.

To help get started, the following is a list of classes commonly employed in the
structuring of dgrid components:

* `dgrid`: Applied to each dgrid list or grid at the top-level element
* `dgrid-header`: Applied to the element which contains the header area
* `dgrid-scroller`: Applied to the element responsible for scrolling the data content
* `dgrid-content`: Applied to the element inside of the scroller area,
  which holds all the data contents
* `dgrid-row`: Applied to each row element
* `dgrid-row-even`: Applied to each even row element
* `dgrid-row-odd`: Applied to each odd row element
  Applying a different color to alternating rows can help visually distinguish individual items.
* `dgrid-selected`: Applied to selected rows or cells
* `dgrid-cell`: Applied to each cell element
* `dgrid-focus`: Applied to the element (cell or row) with the focus (for keyboard based navigation)
* `dgrid-expando-icon`: Applied to the expando icon on tree nodes
* `dgrid-row-expanded`: Applied to rows that have been expanded when using the `Tree` mixin
* `dgrid-header-scroll`: Applied to the node in the top right corner of a Grid,
  above the vertical scrollbar

When `addUiClasses` is set to `true` (the default), the following generic class
names are also available for generic skinning (following the jQuery ThemeRoller convention):

* `ui-widget-content`: Applied to each dgrid list or grid at the top element
* `ui-widget-header`: Applied to the element that contains the header rendering
* `ui-state-default`: Applied to each row element
* `ui-state-active`: Applied to selected rows or cells
* `ui-state-highlight`: Applied to a row for a short time when the contents are changed (or it is newly created)

## The `dgrid-autoheight` class

There are various cases where it is desirable for a grid to adjust its height according to its contents.  dgrid
contains styles for this under the `.dgrid-autoheight` selector.  Making a grid's height size to fit is as simple
as adding the `dgrid-autoheight` class to the DOM node used to create the grid, or passing it via the `className`
property to the instance.

**Note:** Using the `dgrid-autoheight` class with `OnDemandList` is **not** recommended, as `OnDemandList` will
end up ultimately rendering all of the grid's rows anyway, page by page.  `dgrid-autoheight` works well with
`Pagination`.  If you are really interested in rendering all store data at once into an auto-height list or grid,
have a look at our [tutorial](http://dgrid.io/tutorials/0.4/single_query) on the subject.
