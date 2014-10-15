Usage with Visual Studio

It is a simple task to integrate Visual Studio with webpack, all you need to do is use the external tool feature.

## You can add an external tool to the Tools menu. 

1. Open the External Tools dialog box and click Add.
1. Title: field use webpack
1. Command: path to webpack `webpack.cmd` - Assuming webpack was installed globally with npm.
   `C:\Users\{{username}}\AppData\Roaming\npm\webpack.cmd`
1. Arguments: `-w`
1. Check `Use Output window`

![Visual Studio External Tool](http://d3m4lzjblc2qwl.cloudfront.net/webpack-tool.png)

## Now add to your toolbar

1. On the menu bar, right click and select `Customize...`.
1. Click on the `Commands` tab and click on `ToolBar` radio button to select the newly created external tool.
![Visual Studio customize toolbar](http://d3m4lzjblc2qwl.cloudfront.net/customize-toolbar.png)
1. Select Standard and click on `Add Command ...` button.
1. On the left lit item, select `Tools` and than select the `External Command X` item where X is the index of your tool that appears in the `Tools` menu (starting index => 1). In my example `External Command 6`.
![Visual Studio customize toolbar](http://d3m4lzjblc2qwl.cloudfront.net/add-command.png)
1. Click `Ok` and then `Close`.
