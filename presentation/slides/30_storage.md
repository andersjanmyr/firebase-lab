!SLIDE bullets
# Storage

* Store and serve user-generated content, such as photos or videos
* Secure file uploads and downloads for your Firebase apps
* Backed by Google Cloud Storage

!SLIDE code
# Storage References (web)

    @@@ Javascript
    var storageRef = firebase.storage().ref();

    // Create a child reference
    var imagesRef = storageRef.child('images');
    // imagesRef now points to 'images'

    // Child references can also take paths delimited by '/'
    var spaceRef = storageRef.child('images/space.jpg');

    // Properties
    var path = spaceRef.fullPath     // images/space.jpg
    var name = spaceRef.name         // space.jpg
    var imagesRef = spaceRef.bucket; // the bucket
    var imagesRef = spaceRef.parent; //images

~~~SECTION:notes~~~
~~~ENDSECTION~~~

!SLIDE code
# Storage Upload, Put (web)

    @@@ Javascript
    var storageRef = firebase.storage().ref();

    function handleFileSelect(evt) {
        var file = evt.target.files[0];
        var metadata = { 'contentType': file.type };

        // Upload a file and metadata
        storageRef.child('images/' + file.name).put(file, metadata).then(function(snapshot) {
            console.log('Uploaded', snapshot.totalBytes, 'bytes.');
            console.log(snapshot.metadata);
            var url = snapshot.metadata.downloadURLs[0];
            console.log('File available at', url);
        }).catch(function(error) {
            console.error('Upload failed:', error);
        });
    }

~~~SECTION:notes~~~
~~~ENDSECTION~~~

!SLIDE code
# Storage Get URL, Download (web)

    @@@ Javascript
    // Create a reference to the file we want to download
    var starsRef = storageRef.child('images/stars.jpg');

    // Get the download URL
    starsRef.getDownloadURL().then(function(url) {
        // Insert url into an <img> tag to "download"
    }).catch(function(error) {
        switch (error.code) {
          case 'storage/object_not_found':
              break;

          case 'storage/unauthorized':
              break;

          case 'storage/canceled':
              break;

          case 'storage/unknown':
              // Unknown error occurred, inspect the server response
              break;
        }
    });

~~~SECTION:notes~~~
~~~ENDSECTION~~~

!SLIDE code
# Storage Metadata (web)

    @@@ Javascript
    // Create a reference to the file whose metadata we want to retrieve
    var forestRef = storageRef.child('images/forest.jpg');

    // Get metadata properties
    forestRef.getMetadata().then(function(metadata) {
        // Metadata now contains the metadata for 'images/forest.jpg'
        // name, size, contentType, etc.
    }).catch(function(error) {
        // Uh-oh, an error occurred!
    });

~~~SECTION:notes~~~
~~~ENDSECTION~~~


!SLIDE bullets
# Storage Metadata Properties

* Updatable properties:
  - `cacheControl`, `contentEncoding`, `contentType`, `contentLanguage`, `contentDisposition`
  - `customMetadata`
* Immutable properties
  - `bucket`, `fullPath`, `name`, `size`
  - `timeCreated`, `updated`, `md5hash`
  - `generation`, `metageneration`

~~~SECTION:notes~~~
~~~ENDSECTION~~~


!SLIDE code
# Storage

    @@@ Javascript

~~~SECTION:notes~~~
~~~ENDSECTION~~~


!SLIDE code
# Storage

    @@@ Javascript

~~~SECTION:notes~~~
~~~ENDSECTION~~~

