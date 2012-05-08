## jQuery column cell selector

This plugin adds a new selector to the jQuery selector API for retrieving table cells by their column index. It supports tables with column and row spans transparently, no matter how complex your table is. The syntax is simple and similar to other jQuery selectors so there is a very small learning curve. The following example shows how to select all the cells in the fourth column of a table.

    $('#mytable tbody td:nth-col(4)');

The selector can take several types of arguments for selecting columns, such as keywords, numeric indexes, and equations. These are explained in more detail in the API section.

## API

This plugin adds one new pseudo selector to the jQuery selector API, called `nth-col`. Additionally it adds one new method to the jQuery function namespace called `nthCol` (which one to use depends on your use case, please refer to the FAQ section for details.) Both the selector and extension function provide the same functionality and accept the same arguments. The following arguments can be used:

<dl>
    <dt>even</dt>
    <dd>Selects and returns all cells in even columns.</dd>
    
    <dt>odd</dt>
    <dd>Selects and returns all cells in uneven columns.</dd>
    
    <dt>equation</dt>
    <dd>An equation in the form of `a * n + b` , where both `a` and `b` can be negative or positive, and n represents the column index. This will split the columns in groups of size `a` and select the child with position `b` in each group. Both `a` and `b` are optional, but at least one should be provided. The syntax is identical to that of the [CSS nth-child pseudo selector](http://www.w3.org/TR/2009/WD-css3-selectors-20090310/#nth-child-pseudo). Invalid equations are ignored and return no results.
</dd>
    <dt>number</dt>
    <dd>Selects and returns a single column with the specified numeric index. This is a simplified version of the equation syntax.</dd>
</dl>

Note that column numbering starts at one, not at zero as in most programming languages. Please have a look at the examples in the next section to see how the equation syntax is used.

## Examples

The following are all examples of using both the column selector and extension function. Note that the syntax for both is identical, but that equations and keywords passed to the `nthCol` extension function must be properly escaped.

    // select all even cells in the tbody section of #mytable
    $('#mytable tbody td:nth-col(even)');
    
    // same as above, but using the nthCol extension function
    $('#mytable tbody td').nthCol('even');
    
    // selects both table header and table data cells from the third column of #mytable
    $('#mytable th:nth-col(3), #mytable td:nth-col(3)');
    
    // same as above but using the nthCol extension function
    $('#mytable th, #mytable td).nthCol(3);
    
    // select the first element in the third group
    $('#mytable td:nth-col(3n+1)');
    
    // same as above but using the nthCol extension function
    $('#mytable td').nthCol('3n+1');
    
    // select the cells in the 9th, 19th, 29th, etc, column of #mytable
    $('#mytable td:nth-col(10n-1)');

More examples are available on a separate page which shows [examples of column selectors](examples/column-selectors.html) used with tables containing complicated column and row spans.

## Frequently Asked Questions

Q: What is the difference between the selector and the nthCol jQuery extension function?

A: Although both use the same code, the `nthCol` extension function is probably faster. The reason for this is that the selector engine in jQuery uses a bottom up approach. This is a good thing for most selectors, but not so much for a column selector because it needs context information, such as access to rows and table sections ( `thead` , `tbody` and `tfoot`.) For example, say you write the following selector:

    #mytable tbody td:nth-col(even)

The selector engine will first select all `td` elements in your document and pass them to the column selector. This means the column selector will have to calculate the correct column cells for all tables in the document instead of just the ones in `#mytable`. Although it does so in the most efficient way possible, there is still a cost for doing these calculations on tables you did not intend to select. The `nthCol` extension function on the other hand works on the result of the selector, meaning all irrelevant elements have already been thrown out, and as such it does not do any unnecessary work. My recommendation for now is that you start with using the column selector in your code, and only if profiling shows a performance problem switch to the `nthCol` extension function.

Q: Why not just use col and colgroup?

A: There are several reasons you might want to use this plugin over `col` and `colgroup` . First, [col and colgroup are restricted in the CSS attributes they support](http://www.w3.org/TR/CSS21/tables.html#columns), which may be problematic if you want to do advanced styling. They are limited because `col` and `colgroup` define artificial elements that are not actually there in the source document. This plugin on the other hand returns the actual table cells that make up the column. These cells can then be styled in any way. Second, browser support for those `col` and `colgroup` attributes that can be styled is [problematic](http://www.quirksmode.org/css/columns.html), while all browsers fully support styling table cells.

Q: What is the difference between using this plugin and using the standard `#mytable tbody &gt; td:nth-child(n)` selector?

A: For tables that have no row or column span attributes this works just fine, and is the recommended method if you know for sure your tables will never contain a row or column span. If you have row or column spans in your table, or at least the possibility that they might one day be there, you should use the column selector plugin which handles column and row spans correctly.

## Credits

The algorithm used in the column selector was inspired by the work done by [Matt Kruse of The Javascript Toolbox](http://www.javascripttoolbox.com/).

## Alternatives
* [jQuery column iterator](http://slackers.se/2009/jquery-column-iterator-plugin/)