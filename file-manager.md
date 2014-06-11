# File Manager

## Front End

### Raptor Editor

Raptor plugin options:

 - `uriPublic` the URI to the public location of uploads (for links and image sources) 
 - `uriAction` the URI for the file manager actions
 - `uriIcon` the URI for file icons

[Working Raptor PHP example](https://github.com/PANmedia/raptor-example/blob/master/examples/file-manager/example.php) 

Example:

    $('.editable').raptor({
        plugins: {
            fileManager: {
                uriPublic: '/uploads/',
                uriAction: '/actions/file-manager.php',
                uriIcon: '/file-manager/icon/'
            }
        }
    });

### Stand Alone

File Manager options:

 - `node` a DOM node to attach the file manager to 
 - `uriAction` same as above
 - `uriIcon` same as above

    <div id="file-manager"></div>
    <script type="text/javascript">
        var rfm = new RFM({
            node: document.getElementById('file-manager'),
            uriAction: '/actions/file-manager.php',
            uriIcon: '/file-manager/icon/'
        });
    </script>

[Working standalone PHP example](https://github.com/PANmedia/raptor-example/blob/master/examples/file-manager/standalone.php)

### Amazon S3

 - `node` same as above
 - `s3` 
	 - `bucketURL` URL for you Amazon S3 bucket
	 - `accessKeyId` Amazon S3 access key ID
	 - `policy` generated Amazon S3 policy
	 - `signature` signature hash of policy and Amazon S3 secret 

[Example Amazon S3 credentials generation](https://github.com/PANmedia/raptor-example/blob/master/examples/file-manager/amazon-s3-key.php)
 
    <div id="file-manager"></div>
    <script type="text/javascript">
        var rfm = new RFM.S3({
            node: document.getElementById('file-manager'),
            s3: {
                bucketURL: <?= json_encode($buckerUrl); ?>,
                accessKeyId: <?= json_encode($accessKeyId); ?>,
                policy: <?= json_encode($policy); ?>,
                signature: <?= json_encode($signature); ?>
            }
        });
    </script>

[Working (without required credentials) Amazon S3 standalone example](https://github.com/PANmedia/raptor-example/blob/master/examples/file-manager/standalone-amazon-s3.php)


## Back End

### List

Request parameters (GET):

 - `action` must be `list`
 - `path` current path to list (if subdirectories is supported)
 - `start` the start offset
 - `limit` amount of files to return
 - `sort` the sort column:
	 - `name` file name
	 - `size` file size
	 - `type` file type
	 - `mtime` file modified time
 - `tags` comma separated list of tags to filter by
 - `search` search terms to filter by

Example request:

    http://example.com/file-manager.php?action=list&path=%2F&start=0&limit=10&sort=mtime&direction=desc&tags=&search=

Response parameters:

 - `start` same as request
 - `limit` same as request
 - `total` total files in the current directory
 - `filteredTotal` total files in the current directory after applying filters
 - `tags` list of available tags to filter by
 - `directories` list of subdirectories in the current directory
 - `files` list of files in the current directory
	 - `name` file name
	 - `attributes` custom attributes
	 - `type` file type
	 - `size` file size (in bytes)
	 - `mtime` file modified time (seconds since epoch)
	 - `tags` list of tags for the file

Example response:

	{
	    "start": "0",
	    "limit": 10,
	    "total": 21,
	    "filteredTotal": 21
	    "tags": [
	        "Image",
	        "Document",
			...
	    ],,
	    "directories": [
	        "some-directory",
	        "another-directory",
			...
	    ],
	    "files": [
	        {
	            "name": "foo.png",
	            "attributes": {
	                "alt": "Foo alt text"
	            },
	            "type": "png",
	            "size": 12748,
	            "mtime": 1402453369,
	            "tags": [
	                "Image"
	            ]
	        },
	        {
	            "name": "bar.png",
	            "attributes": {
	                "alt": "Bar alt text"
	            },
	            "type": "png",
	            "size": 15500,
	            "mtime": 1402453369,
	            "tags": [
	                "Image"
	            ]
	        },
			...
	    ]
	}

[Example PHP action to list files](https://github.com/PANmedia/raptor-example/blob/master/classes/RFM/ActionList.php)

### Upload

[Plupload](http://www.plupload.com/) is used to handle file uploads on the client side.

Request parameters:

 - `action` must be `upload`

Examples:

 - [Example PHP action to upload files](https://github.com/PANmedia/raptor-example/blob/master/classes/RFM/ActionUpload.php)
 - [Official Plupload PHP example](https://github.com/moxiecode/plupload/blob/master/examples/upload.php)

### Delete

Request parameters (POST):

 - `action` must be `delete`
 - `path` path to file

### Rename

Request parameters (POST):

 - `action` must be `rename`
 - `path` path to file
 - `name` new file name

### Icon

Specifying an icon end point allows the back end to resize images on demand.

Example Request:

    http://example.com/file-manager/icons/foo.png

Example back end to resize icons:

- [Apache rewrite URL handler](https://github.com/PANmedia/raptor-example/blob/master/examples/file-manager/icon/file/.htaccess)
- [PHP image resizer](https://github.com/PANmedia/raptor-example/blob/master/examples/file-manager/icon/file/index.php)
     
## Notes

*Subdirectory support is still experimental.*

*Provided examples are not considered production ready. They lack basic authentication checks, and are not optimal for high load servers.* 