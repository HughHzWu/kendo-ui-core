---
title: Exporting to PDF
page_title: Exporting to PDF | Kendo UI Grid Widget
description: "Learn how to set the PDF export functionality of the Kendo UI Grid widget."
slug: exporting_pdf_kendoui_grid_widget
position: 8
---

# Exporting to PDF

As of Kendo UI Q3 2014 (2014.3.1119) onwards, the Grid widget provides a built-in PDF export functionality.

## Set up

To enable PDF export, include the corresponding toolbar command and configure the export settings.

* [Toolbar Configuration](/api/javascript/ui/grid#configuration-toolbar)
* [PDF Export Configuration](/api/javascript/ui/grid#configuration-pdf)
* [Online Demo](http://demos.telerik.com/kendo-ui/grid/pdf-export)

It's highly recommended that you include the Pako Deflate library on the page to enable compression.

###### Example - enable PDF export

```html
    <!-- Load Pako Deflate library to enable PDF compression -->
    <script src="http://kendo.cdn.telerik.com/{{ site.cdnVersion }}/js/pako_deflate.min.js"></script>

    <div id="grid"></div>
    <script>
        $("#grid").kendoGrid({
            toolbar: ["pdf"],
            pdf: {
                fileName: "Kendo UI Grid Export.pdf"
            },
            dataSource: {
                type: "odata",
                transport: {
                    read: "http://demos.telerik.com/kendo-ui/service/Northwind.svc/Products"
                },
                pageSize: 7
            },
            sortable: true,
            pageable: true,
            columns: [
                { width: 300, field: "ProductName", title: "Product Name" },
                { field: "UnitsOnOrder", title: "Units On Order" },
                { field: "UnitsInStock", title: "Units In Stock" }
            ]
        });
    </script>
```

To initiate PDF export via code, call the [`saveAsPdf`](/api/javascript/ui/grid.html#methods-saveAsPdf) method.

> **Important**
> * By default, Kendo UI Grid exports the current page of the data with sorting, filtering, grouping, and aggregates applied.
> * The Grid uses the current column order, visibility, and dimensions to generate the PDF file.

## Features

### Export all Pages

By default, the Kendo UI Grid exports only the current page of data. To export all pages set the [`allPages`](/api/javascript/ui/grid#configuration-pdf.allPages) option to `true`.

> **Important**
> When the `allPages` option is set to `true` and `serverPaging` is enabled, the Grid will make a `"read"` request for all data. If the data items are too many the browser may become unresponsive. Consider implementing server-side export for such cases.

###### Example - export all pages

```html
    <div id="grid"></div>
    <script>
        $("#grid").kendoGrid({
            toolbar: ["pdf"],
            pdf: {
                allPages: true
            },
            dataSource: {
                type: "odata",
                transport: {
                    read: "http://demos.telerik.com/kendo-ui/service/Northwind.svc/Products"
                },
                pageSize: 7
            },
            pageable: true,
            columns: [
                { width: 300, field: "ProductName", title: "Product Name" },
                { field: "UnitsOnOrder", title: "Units On Order" },
                { field: "UnitsInStock", title: "Units In Stock" }
            ]
        });
    </script>
```

### Server Proxy

Internet Explorer 9 and Safari do not support the option for saving a file and require the implementation of a [server proxy](/framework/save-files/introduction#browser-support). Set the [`proxyURL`](/api/javascript/ui/grid#configuration-pdf.proxyURL) option to specify the server proxy URL.

###### Example - using server proxy

    <div id="grid"></div>
    <script>
        $("#grid").kendoGrid({
            toolbar: ["pdf"],
            pdf: {
                fileName: "Kendo UI Grid Export.pdf",
                proxyURL: "/proxy"
            },
            dataSource: {
                type: "odata",
                transport: {
                    read: "http://demos.telerik.com/kendo-ui/service/Northwind.svc/Products"
                },
                pageSize: 7
            },
            sortable: true,
            pageable: true,
            columns: [
                { width: 300, field: "ProductName", title: "Product Name" },
                { field: "UnitsOnOrder", title: "Units On Order" },
                { field: "UnitsInStock", title: "Units In Stock" }
            ]
        });
    </script>


### Save Files on Server

In some cases it is useful to send the generated file to a remote service. Do this by setting a proxyUrl and forceProxy to true.

If the proxy returns "204 No Content" no file will be sent to the server.

###### Example - post files to the server

    <div id="grid"></div>
    <script>
        $("#grid").kendoGrid({
            toolbar: ["pdf"],
            pdf: {
                forceProxy: true,
                proxyURL: "/proxy"
            }
            dataSource: {
                type: "odata",
                transport: {
                    read: "http://demos.telerik.com/kendo-ui/service/Northwind.svc/Products"
                },
                pageSize: 7
            },
            pageable: true,
            columns: [
                { width: 300, field: "ProductName", title: "Product Name" },
                { field: "UnitsOnOrder", title: "Units On Order" },
                { field: "UnitsInStock", title: "Units In Stock" }
            ]
        });
    </script>

### Embedding Custom Fonts (Unicode Support)

The default fonts in PDF files do not support Unicode.
In order to support international characters we need to embed an external font.

We ship the [Deja Vu font family](http://dejavu-fonts.org/wiki/Main_Page) as part of Kendo UI.
See [Custom Fonts and PDF](/framework/drawing/drawing-dom.html#custom-fonts-and-pdf) for details.

###### Example - Custom Font

```html
    <style>
        /*
            Use the DejaVu Sans font for display and embedding in the PDF file.
            The standard PDF fonts have no support for Unicode characters.
        */
        .k-grid {
            font-family: "DejaVu Sans", "Arial", sans-serif;
        }
    </style>

    <script>
        // Import DejaVu Sans font for embedding

        // NOTE: Only required if the Kendo UI stylesheets are loaded
        // from a different origin, e.g. kendo.cdn.telerik.com
        kendo.pdf.defineFont({
            "DejaVu Sans"             : "http://kendo.cdn.telerik.com/{{ site.cdnVersion }}/styles/fonts/DejaVu/DejaVuSans.ttf",
            "DejaVu Sans|Bold"        : "http://kendo.cdn.telerik.com/{{ site.cdnVersion }}/styles/fonts/DejaVu/DejaVuSans-Bold.ttf",
            "DejaVu Sans|Bold|Italic" : "http://kendo.cdn.telerik.com/{{ site.cdnVersion }}/styles/fonts/DejaVu/DejaVuSans-Oblique.ttf",
            "DejaVu Sans|Italic"      : "http://kendo.cdn.telerik.com/{{ site.cdnVersion }}/styles/fonts/DejaVu/DejaVuSans-Oblique.ttf"
        });
    </script>

    <!-- Load Pako ZLIB library to enable PDF compression -->
    <script src="//kendo.cdn.telerik.com/{{ site.cdnVersion }}/js/pako_deflate.min.js"></script>

    <div id="grid"></div>
    <script>
        $("#grid").kendoGrid({
            toolbar: ["pdf"],
            pdf: {
                allPages: true
            },
            dataSource: {
                type: "odata",
                transport: {
                    read: "http://demos.telerik.com/kendo-ui/service/Northwind.svc/Products"
                },
                pageSize: 7
            },
            pageable: true,
            columns: [
                { width: 300, field: "ProductName", title: "Product Name" },
                { field: "UnitsOnOrder", title: "Units On Order" },
                { field: "UnitsInStock", title: "Units In Stock" }
            ]
        });
    </script>
```

### Limitations

All [Known Limitations](/framework/drawing/drawing-dom#known-limitations) of the HTML Drawing module apply. Most importantly:

* Right-to-left text is not supported
* Images hosted on different domains might not be rendered, unless permissive [Cross-Origin HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_enabled_image) are provided by the server.  Similarly, fonts might not be possible to load cross-domain.

  Even with the proper CORS headers, IE9 will *not* be able to load images or fonts from another domain, and could raise an uncatcheable security exception.  If you need to support IE9, make sure to host images and fonts on the same domain as the application.

* Maximum document size is limited to 5080x5080mm (200x200 inches) by the PDF 1.5 specification. Larger files might not open in all viewers.

* Older browsers, such as Internet Explorer 9 and Safari, requires the implementation of a server proxy. For more information on this, refer to [the `proxyUrl` configuration section](/api/javascript/ui/grid#configuration-pdf.proxyURL).

* PDF export is not supported in Internet Explorer 8 and older.

## Further Reading

* [Drawing HTML](/framework/drawing/drawing-dom)
* [Export MVC Grid to PDF](https://github.com/telerik/ui-for-aspnet-mvc-examples/tree/master/grid/pdf-export-server-side)
* [Export MVC Grid to CSV](https://github.com/telerik/ui-for-aspnet-mvc-examples/tree/master/grid/csv-export-server-side)
* [Save Files with Kendo UI](/framework/save-files/introduction)

## See Also

Other articles on Kendo UI Grid:

* [JavaScript API Reference](/api/javascript/ui/grid)
* [Walkthrough of the Grid]({% slug walkthrough_kendoui_grid_widget %})
* [Remote Data Binding]({% slug remote_data_binding_grid %})
* [Editing Functionality]({% slug editing_kendoui_grid_widget %})
* [Localization of Messages]({% slug localization_kendoui_grid_widget %})
* [Adaptive Rendering]({% slug adaptive_rendering_kendoui_grid_widget %})
* [Printing Your Grid]({% slug printing_kendoui_grid %})