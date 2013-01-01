Prototype.Tabs
==============

Mirrored from http://roland.devarea.nl/work/tabs/

Create javascript tabs from an unordered list with the ability to dynamically load tab contact via Ajax



given this HTML
```html
<div id="tabs">
        <ul class="tabs">
                <li><a href="#tab-link" rel="target_1">Tab 1</a></li>
                <li><a href="#tab-link" rel="target_2">Tab 2</a></li>
                <li><a href="#tab-link" rel="target_3">Tab 3</a></li>
                <li><a href="#tab-ajax" rel="ajax">Tab 4</a></li>
        </ul>
        <div id="target_1">
                Tab 1 target
        </div>
        <div id="target_2">
                Tab 2 target
        </div>
        <div id="target_3">
                Tab 3 target
        </div>
</div>
<a href="javascript:void(0);" id="add_tab">Add tab</a> - 
<a href="javascript:void(0);" id="del_tab">Delete last tab</a> - 
<a href="javascript:void(0);" id="get_tab">Get active tab</a> - 
<a href="javascript:void(0);" id="open_tab">Open next tab</a>
<div id="console"><!-- --></div>
```

You can use this Javascript to setup the tabs
```javascript
var MyTabs = new Tabs('tabs', {
        start: Tabs.LAST,
        callback: {
                afterOpen: function(panel) {
                        $('console').update(
                                'Panel index: ' + panel.INDEX + '<br />' +
                                'Inner HTML LI element: ' + panel.LI.innerHTML.escapeHTML().strip() + '<br />' +
                                'Rel attribute link element: ' + panel.LINK.readAttribute('rel') + '<br />' +
                                'Inner HTML content element: ' + panel.CONTENT.innerHTML.escapeHTML().strip()
                        );
                },
                isEmpty: function() {
                        $('console').update('Tab panel is empty...');
                }
        },
        // ajax: 'ajax.php',
        ajax: function(panel) {
                return panel.LINK.readAttribute('rel') == 'ajax'
                        ? 'ajax.php'
                        : null;
        },
        ajaxCache: false
});

$('add_tab').observe('click', function() {
        MyTabs.addPanel('Label', 'Some static content...'); // add 3rd parameter set to false to disable auto-focus
});
$('del_tab').observe('click', function() {
        MyTabs.deletePanel(Tabs.LAST); // Tabs.FIRST | Tabs.ACTIVE | Tabs.LAST | Tabs.NEXT | Tabs.PREV | Index integer
});
$('get_tab').observe('click', function() {
        var tab;
        if(!!(tab = MyTabs.panelByIndex(Tabs.ACTIVE)))
                $('console').update('Panel index ' + tab.INDEX + ' fetched.');
});
$('open_tab').observe('click', function() {
        MyTabs.openPanel(Tabs.NEXT);
});
```

and a simple backend script

```php
<?php
sleep(3);
echo 'Server date: ' . date('d-m-Y H:i:s');
```