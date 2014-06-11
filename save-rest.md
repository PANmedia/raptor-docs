# Saving Content with REST

Configuring Raptor to use the `saveRest` plugin.


	$('.editable').raptor({
	    plugins: {
	        // The save UI plugin/button
	        save: {
	            // Specifies the UI to call the saveRest plugin to do the actual saving
	            plugin: 'saveRest'
	        },
	        saveRest: {
	            // The URI to send the content to
	            url: '/path/to/save',
	            // Returns an object containing the data to send to the server
	            data: function(html) {
	                return {
	                    id: this.raptor.getElement().data('id'),
	                    content: html
	                };
	            }
	        }
	    }
	});

## Example HTML

A very simple example of HTML that this technique might be applied to.

    <div class="editable" data-id="header">
        <h1>The header content</h1>
    </div>
    <div class="editable" data-id="body">
        <p>The body content</p>
    </div>

## Example Request

Using this example the browser should send 2 separate requests to `http://localhost:3000/path/to/save` contain a `id` and `content` POST parameter. 

The `id` parameter will correspond to the `data-id` attribute in the HTML.

### Example PHP Handler

    if (isset($_POST['id']) && isset($_POST['content'])) {
        $stmt = $pdo->prepare("
			REPLACE INTO table (id, content) 
			VALUES (:id, :content);
		");
		$stmt->execute([
			':id' => $_POST['id'],
			':content' => $_POST['content'],
		]);
    }

### Example Rails Controller

The following Rails code describes how you might access and save the content.

    class ContentController < ApplicationController
      respond_to :json

      def update
        @content = Content.find(params[:id])
        @content.update_attributes(params[:content])
        respond_with @content
      end
    end

### See Also

- [Raptor Rails Gem](https://github.com/PANmedia/raptor-editor-rails)
- [Raptor example code on GitHub.](https://github.com/PANmedia/raptor-example/)