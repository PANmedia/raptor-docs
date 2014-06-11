# Events

Raptor has custom events that occur when editing. You can bind to event using initialisation options, or when developing plugins.

## Options Example

This example loads the source of the block from a data base 64 encoded attribute before enabling editing, then on save it puts the new source back into the data attribute.

    $('.editable').raptor({
        bind: {
            enabling: function() {
                var element = this.getElement(),
                    source = element.attr('data-source');
                if (source) {
                    element.html(atob(source));
                }
            },
            saved: function() {
                var html = this.getHtml();
                this.getElement()
                    .attr('data-source', btoa(html));
            }
        }
    });

## Plugin Example

This example is taken from [this dock plugin](https://github.com/PANmedia/raptor-editor/tree/master/src/plugins/dock/dock-plugin.js).

    DockPlugin.prototype.init = function() {
        ...
        this.raptor.bind('toolbarReady', function() {
            if (docked) {
                this.toggleState();
            }
        }.bind(this));
		...
	}

## Event List

	cancel -- Triggered when cancelling editing
	change -- Triggered after a change to the block source is made
	cleaned -- Triggered when the source code is the same as it's original source code
	destroy -- Triggered when Raptor is destroyed (this is when plugins should clean up)
	dirty -- Triggered when the source code is edited for the first time (opposite of clean)
	disabled -- Triggered when editing is disabled
	enabled -- Triggered when editing is enabled
	enabling -- Triggered just before editing is enabled, which allows modification of the source before editing
	historyChange -- Triggered when the history (undo/redo) is changed
	html -- Triggered the the source of the block is set via raptor.setHtml(html)
	layoutHide -- Triggered to alert all layouts to hide themselves
	layoutReady -- Triggered by a layout when it is ready
	layoutShow -- Triggered to alert all layouts to show themselves
	ready -- Triggered when Raptor is ready
	resize -- Triggered when something resizes which indicates layouts/plugins should adjust accordingly
	saved -- Triggered on successful save
	selectionChange -- Triggered when the select is changed
	selectionCustomise -- Triggered before applying an action which allows plugins to provide a custom selection (such as only the table column content rather than an entire row)
