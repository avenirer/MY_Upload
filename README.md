# MY_Upload

This library is an extension of the CI_Upload library that will allow you to upload multiple files.

## How to install

Just move libraries/MY_Upload.php to your application/libraries/ directory.

## How to use it:

First of all, let's make the view for uploading multiple files:

```php
<h1>Upload multiple files</h1>
<?php echo form_open_multipart();?>
<p>Upload file(s):</p>
<?php echo form_upload('uploadedimages[]','','multiple'); ?>
<br />
<br />
<?php echo form_submit('submit','Upload');?>
<?php echo form_close();?>
```

Now, in controller, if a form was submited, we set the configuration and load the library:

```php
$config = array(
	'upload_path' => './upload/',
	'allowed_types' => 'jpg|gif',
	'max_size' => '2048',
	'multi' => 'all'
);

// load Upload library
$this->load->library('upload',$config);
```

As you can see, the $config array has a new key: "multi". This key can accept one of three values:

- 'all' = only upload if all files are uploaded
- 'halt' = halt uploading after first encountered error
- 'ignore' = ignore errors and continue uploading remaining files

Once we've loaded the library, we can simply call `do_upload()` on our input element:

```php
$this->upload->do_upload('uploadedimages');
```

Take care here: `$this->upload->do_upload()` returns FALSE only if an error was encountered AND:

- either the user tried to upload only one file;
- or the user tried to load more than one file BUT the $config['multi'] was set to 'all'.

After the upload, we can retrieve the data of the uploaded files with $this->upload->data():

```php
echo '<pre>';
$uploaded = $this->upload->data();
print_r($uploaded);
echo '</pre>';
```

Also, in case of upload errors, we can retrieve the errors with $this->upload->display_errors:

```php
echo '<pre>';
print_r($this->upload->display_errors());
echo '</pre>';
```

Enjoy!
