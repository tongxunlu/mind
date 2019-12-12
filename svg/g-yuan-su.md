# g元素

SVG中`g`元素被用来将图形进行分组。一旦分组，你可以把它当作一个单一的形状，对整个图形组进行转换。与嵌套的[svg元素](https://brucewar.gitbooks.io/svg-tutorial/SVG-svg%E5%85%83%E7%B4%A0.md)相比，将元素作为整体转换是它的一个优点。

并且，`<g>`元素上设置的CSS样式将会被其子元素继承。

相比于将分组形状嵌套在`<svg>`元素中，将分组形状嵌套在`<g>`元素中的优点是能对所有的图形进行转换。`<svg>`元素不能自己转换，为了转换嵌套的图形，你不得不将`<svg>`元素嵌套在`<g>`元素中。

与`<svg>`元素相比，`<g>`元素也有一个缺点。你无法通过改变`<g>`元素的x和y属性来移动包含所有嵌套形状`<g>`元素。`<g>`元素没有x和y属性。如果想这么做，你只能使用`transform`属性来移动`<g>`元素，并使用“translate”函数，就像这样：`transform="translate(x,y)"`。

如果你想要使用x和y属性来移动`<g>`元素中的所有图形，你需要将`<g>`元素嵌套在`<svg>`元素中。


