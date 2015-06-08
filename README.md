# react-designer

React designer is WYSIWYG editor for **easy content creation** (legal contracts, business forms, marketing leaflets, awesome inforgrafics, technical guides, visual reports, rich dashboards, tutorials and other content, etc.).
The documents created are called as [reactive-document](http://hand-formvalidation.rhcloud.com), because of its dynamic substance. Reactive document ensures the user experience of paper and pan and enables document to adapt to user actions and its preferences.
The content can be published to user via [react-publisher](https://github.com/rsamec/react-publisher).

It is using [react](https://facebook.github.io/react/) framework for rendering documents.

**Warning** - it is prototype and work in progress.

![designerScreen](https://github.com/rsamec/react-designer/blob/master/designer_roses.jpg)

## Demo

[DEMO](http://hand-formvalidation.rhcloud.com/designer)

## Features

+   directly manipulate the layout of a document without having to type or remember names of components, elements, properties or other layout commands. 
+   high-quality on-screen output and on-printer output
+   precise visual layout that corresponds to an existing paper version
    +   support for various output formats - html, pdf, etc. 
+   support for various position schemas -> absolute, relative position of elements (normal flow is not yet implemented)
+   comfortable user experience - basic WYSIWYG features
    +   support drag nad drop - resize object length, move object positions
    +   support manipulating objects -> copy, move, up, down objects in object schema hierarchy
    +   highlighting currently selected object and its parent
+   possible to use build-in editors 
    +   html editors - html fragments  - using [TinyMceEditor]
    +   json editors - json configuration, data 
    +   JSX editor -  custom react component - JSX editor - using [code-mirror]
    +   color picker - select color from color palette
    +   test data generator - enables to generate test data - useful for charts
    +   other standard basic editors - number, date, select editors
+   support for inspectors (html, json inspector)
+   search and filter options
+   toolbox can be extensible with any widget (react component) - hundreds of [react-components] or create your custom component (even directly in designer)
+   undo/redo functionality
+   build-in html content publishing (preview of html dynamic document)
+   usable for big documents - careful designed to use react performance
    +   we won't render the component if it doesn't need it
    +   simple comparison is fast because of using immutable data structure
+   simple document description (json) that enables to easy map to react component and its properties     

## Reactive document object model (RDOM)

RDOM is object schema format that represents full description of document.

It is a simple object tree that consists of

+   **containers** - nodes that are invisible components - usable for logical grouping of reactive parts of document (sections)
+   **boxes** - terminal nodes (leaf) that are visible components - (react components, boxes, widgets) - it maps to props of react component

Example RDOM document

```json

{ 
 "name": "rootContainer",
 "elementName": "ObjectSchema",
 "containers": [ 
    { 
     "name": "container",
     "elementName": "Container",
     "style": { "top": 0, "left": 0, "height": 200, "width": 740, "position": "relative" }
     "boxes": [{ 
        "name": "TextBox",
        "elementName": "TextBox",
        "style": { "top": 0, "left": 0 },
        "content": "Type your text" 
        }],
      "containers": [ ] }
    ]
}

```

You can see 2 collections (arrays) of objects

+   containers - collection of children
+   boxes - collection of widgets

The object schema tree is composed using __containers__ property as collection of children.
The boxes on the other hand is a leaf collection that can not have other children.

See these object properties

+   elementName - type of element
+   style - element positions and dimensions
    +   top, left - element position
    +   width, height - element dimension
+   containers - collection of children elements (only if there are any children containers)
+   boxes - collections of widgets (only if there are any boxes)

All other properties are optional and are typically specific for widgets as widget's options.

### React elements and components

Typically widget object is a react component with props as component's options.

To render in react is super simple 

```js
    createComponent: function (box) {
        var widget =widgets[box.elementName];
        if (widget === undefined) return React.DOM.span(null,'Component ' + box.elementName + ' is not register among widgets.');

        return React.createElement(widget,box, box.content!== undefined?React.DOM.span(null, box.content):undefined);
    },
    render:function(){
       {this.props.boxes.map(function (box, i) {
                var component = this.createComponent(box);
                return (
                       <div style={box.style}>
                            {component}
                       </div>
                       );
       }, this)}
    }
    
    
```


# Designer components

react-designer consists of these parts

Left panel

+   Workplace
    +   Container and Repeater Container - components that can have children
    +   Box - leaf node (can not have children)
        +   CheckBoxInput
        +   ImageBox
        +   TextBox
        +   TextBoxInput
        +   ValueBox
        +   other react components
+   WorkplaceToolbar
+   JsonViewer

Right panel

+   ObjectToolbar
+   ObjectBrowser - tree of objects
+   PropertyGrid
+   Palette - components toolbox
+   Modal
+   Editors
    +   BoolEditor
    +   ColorPickerEditor
    +   JsonEditor
    +   DropDownEditor
    +   HtmlEditor - TinyMceEditor
    +   NumberInputEditor



# Content publishing 
 
There are many ways how you can publish content created in react-designer.

There is basic build-in content publisher in the form of dynamic html document.
There is basic build-in service for content publishing HTML or PDF output (API) (partially implemented in PDF kit).

Feel free to write your own content publisher - see list of some ideas.

+   PDFRenderer - provide high-quality printed output on a variety of printers
    +   SimplePDFRenderer - transforms directly to PDF
    +   HmtlToPDFRederer - transforms to PDF from HTML
+   HtmlRenderer - provide high-quality on-screen output
    +   InputRenderer - allow the user to fill in data in document
        +   SimpleInputRenderer - usage of simple custom controls
        +   BootstrapInputRenderer - usage of react bootstrap components
        +   MaterialUIRenderer - usage of material-ui components
    +   PrintRenderer - Allow the user to visualize what the document will look like when printed
+   some wizards publishers
+   some MDI publishers


# Get started

Run watch task and begin to develop your React components.

```
$ npm install
```

```
$ bower install
```

```
$ gulp dev
```

```
$ gulp pro
```


# Road map

Coming soon

+   exact specification of RDOM format
+   cleaning code, refactoring widgets
+   examples 
    +   add renderer examples (8)
    +   add api examples (?)
    +   add document examples (8)
    +   add examples how to add widgets (8)
+   add more widgets
    +   material-ui
+   improve designer experience
    +   move objects in object browser
    +   improve - resize - when resizing show resize borders (remove temporary hot fix)    
    +   disabled add widget when box is selected (1)
+   performance issues
    +   recheck - should component update
    +   parse property values (parseInt,etc.) - to many places - remove defensive programming favor contract by design
+   consider support for continues saving


Long run

+   full support for css positioning schemas - absolute, relative, normal flow
+   improve current editors + add json editor
+   PDF - better support
    +   publish pdfkit service 
    +   better support html fragments -> to pdf (using html parser) - consider using pdfmake
+   support for svg - rendering on server


# Licence

MIT Copyright (c) 2015 Roman Samec
