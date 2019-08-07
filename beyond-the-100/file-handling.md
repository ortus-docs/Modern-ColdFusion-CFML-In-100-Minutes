# File Handling

CFML allows you to manipulate, read, upload, etc files via its built in methods which are great and easy to use. It can even help you manipulate zip/jar archives!  We won't go into every single detail of file handling, but below you can find the majority of functions to deal with file handling. 

{% hint style="success" %}
You can find the file system functions here: [https://cfdocs.org/filesystem-functions](https://cfdocs.org/filesystem-functions).
{% endhint %}

* [DirectoryCopy](https://docs.lucee.org/reference/functions/directorycopy.html) Copies the contents of a directory to a destination directory.
* [DirectoryCreate](https://docs.lucee.org/reference/functions/directorycreate.html) Creates new directory for specified path
* [DirectoryDelete](https://docs.lucee.org/reference/functions/directorydelete.html) Deltes directory for given path
* [DirectoryExists](https://docs.lucee.org/reference/functions/directoryexists.html) Determines whether a directory exists.
* [DirectoryList](https://docs.lucee.org/reference/functions/directorylist.html) Lists the directory and returns the list of files under it as array or query
* [DirectoryRename](https://docs.lucee.org/reference/functions/directoryrename.html) Renames given directory
* [ExpandPath](https://docs.lucee.org/reference/functions/expandpath.html) Creates an absolute, platform-appropriate path that is equivalent to the value of relative\_path, appended to the base path. This function \(despite its name\) can accept an absolute or relative path in the relative\_path attribute
* [FileAppend](https://docs.lucee.org/reference/functions/fileappend.html) appends the entire content to the specified file.
* [FileClose](https://docs.lucee.org/reference/functions/fileclose.html) Closes an file that is open.
* [FileCopy](https://docs.lucee.org/reference/functions/filecopy.html) Copies the specified on-disk or in-memory source file to the specified destination file.
* [FileDelete](https://docs.lucee.org/reference/functions/filedelete.html) Deletes the specified file on the server.
* [FileExists](https://docs.lucee.org/reference/functions/fileexists.html) Determines whether a file exists
* [FileGetMimeType](https://docs.lucee.org/reference/functions/filegetmimetype.html) Returns the mimetype of the given file
* [FileInfo](https://docs.lucee.org/reference/functions/fileinfo.html) returns detailed info about the given file.
* [FileIsEOF](https://docs.lucee.org/reference/functions/fileiseof.html) Determines whether Lucee has reached the end of the file while reading it.
* [FileMove](https://docs.lucee.org/reference/functions/filemove.html) Moves file from source to destination
* [FileOpen](https://docs.lucee.org/reference/functions/fileopen.html) Opens an file to read, write, or append.
* [FileRead](https://docs.lucee.org/reference/functions/fileread.html) Reads an on-disk or in-memory text file or a file object created with the FileOpen function.
* [FileReadLine](https://docs.lucee.org/reference/functions/filereadline.html) Reads a line from an file.
* [FileSeek](https://docs.lucee.org/reference/functions/fileseek.html) Shifts the file pointer to the given position. The file must be opened with seekable option
* [FileSetAccessMode](https://docs.lucee.org/reference/functions/filesetaccessmode.html) Sets the attributes of an on-disk file on UNIX or Linux. This function does not work with in-memory files.
* [FileSetAttribute](https://docs.lucee.org/reference/functions/filesetattribute.html) For the given path, sets the file attributes.
* [FileSetLastModified](https://docs.lucee.org/reference/functions/filesetlastmodified.html) For the given file, set the last modification date
* [FileSkipBytes](https://docs.lucee.org/reference/functions/fileskipbytes.html) Shifts the file pointer by the given number of bytes.
* [FileTouch](https://docs.lucee.org/reference/functions/filetouch.html) Touches given file, creates the file if not already exists.
* [FileUpload](https://docs.lucee.org/reference/functions/fileupload.html) Uploads file to a directory on the server.
* [FileUploadAll](https://docs.lucee.org/reference/functions/fileuploadall.html) Uploads file to a directory on the server.
* [FileWrite](https://docs.lucee.org/reference/functions/filewrite.html) If you specify a file path, writes the entire content to the specified file. If you specify a file object, writes text or binary data to the file object.
* [FileWriteLine](https://docs.lucee.org/reference/functions/filewriteline.html) Opens up the file \(or uses the existing file object\) and appends the given line of text
* [GetFileInfo](https://docs.lucee.org/reference/functions/getfileinfo.html) Retrieves information about file.
* [GetFreeSpace](https://docs.lucee.org/reference/functions/getfreespace.html) Returns the number of unallocated bytes in the partition named by this abstract path name.
* [GetTempDirectory](https://docs.lucee.org/reference/functions/gettempdirectory.html) Returns the full path to the currently assigned temporary directory
* [GetTempFile](https://docs.lucee.org/reference/functions/gettempfile.html) Creates a temporary file in a directory whose name starts with \(at most\) the first three characters of prefix.
* [ImageWrite](https://docs.lucee.org/reference/functions/imagewrite.html) Writes a image to the specified filename or destination.

```java
// A few examples
content = fileRead( expandPath( "/config/myfile.txt" ) );
if( fileExists( "filepath.txt" ) ){

}
fileDelete( "filepath.txt" )
fileWrite( getTempFile( getTempDirectory(), "tempFile"), "My Data" );

directoryList( "/my/path" )
directoryExists( "/my/path" )

<form method="post" enctype="multipart/form-data">
  <input type="file" name="fileInput">
  <button type="submit">Upload</button>
</form>

<cfscript>
  if( structKeyExists( form, "fileInput" )) {
    try {
      uploadedFile = fileUpload( getTempDirectory(), "fileInput", "image/jpeg,image/pjpeg", "MakeUnique" );
      // check the file extension of the uploaded file; mime types can be spoofed
      if (not listFindNoCase("jpg,jpeg", cffile.serverFileExt)) {
      throw("The uploaded file is not of type JPG.");
      }
      // do stuff with uploadedFile...
    } catch ( coldfusion.tagext.io.FileUtils$InvalidUploadTypeException e ) {
      writeOutput( "This upload form only accepts JPEG files." );
    }
    catch (any e) {
      writeOutput( "An error occurred while uploading your file: #e.message#" );
    }
  }
</cfscript>

```

## Dealing With Large Files

If you want to read or manipulate large files we would recommend that you leverage our [cbStreams](https://forgebox.io/view/cbstreams) library or native Java file streaming.  Below you can find some sample usage of reading large files with [cbStreams](https://forgebox.io/view/cbstreams) which implements the Java Streams API.

```java
stream = streamBuilder.new().ofFile( absolutePath );
try{
    work on the stream of lines of files and close it in the finally block;
} finally{
    stream.close();
}

//You can even process file lines concurrently
stream = streamBuilder.new()
    .parallel()
    .ofFile( absolutePath );
```





