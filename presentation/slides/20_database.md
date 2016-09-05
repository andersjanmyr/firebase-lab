!SLIDE bullets
# Database

* Cloud Hosted
* Realtime, synced to all connected clients
* Offline, write to the database are saved and synced when online
* Stored format is a JSON tree

!SLIDE bullets
# Database Sync (Reading)

* Listen to the events of a specific path in the JSON tree
  - `value` - Read and listen for changes to the entire contents of a path.
  - `child_added`, `child_removed` `child_changed` - Monitor changes to lists.
  - `child_moved` - Fires when a child is changed in away that changes sorting
    order.
* No querying for data, just listen to events

!SLIDE code
# Example List (web)

    @@@ Javascript
    // Get a reference to a post
    var commentsRef = firebase.database().ref('post-comments/' + postId);

    // Listen for comment changes
    commentsRef.on('child_added', function(data) {
      addCommentElement(postElement, data.key, data.val().text, data.val().author);
    });

    commentsRef.on('child_changed', function(data) {
      setCommentValues(postElement, data.key, data.val().text, data.val().author);
    });

    commentsRef.on('child_removed', function(data) {
      deleteComment(postElement, data.key);
    });

!SLIDE code
# Example Value (web)

    @@@ Javascript
    // Get a reference to a value
    var starCountRef = firebase.database().ref('posts/' + postId + '/starCount');

    // Listen for changes
    starCountRef.on('value', function(snapshot) {
        updateStarCount(postElement, snapshot.val());
    });

!SLIDE bullets
# Database Writing

* `set()` - Write or replace data to a defined path, such as `users/<user-id>/<username>`.
* `push()` - Add to a list of data. Every time you call `push()`, Firebase   generates a unique key that can also be used as a unique identifier, such as `user-posts/<user-id>/<unique-post-id>`.
* `update()` - Update some of the keys for a defined path without replacing all of the data.
* `transaction()` - Update complex data that could be corrupted by concurrent updates.

!SLIDE code
# Database Writing, Set (web)

    @@@ Javascript
    function writeUserData(userId, name, email, imageUrl) {
        firebase.database().ref('users/' + userId).set({
            username: name,
            email: email,
            profile_picture : imageUrl
        });
    }

~~~SECTION:notes~~~
Using set() overwrites data at the specified location, including any child nodes.
~~~ENDSECTION~~~

!SLIDE code
# Database Writing, Push (web)

    @@@ Javascript
    function writeNewPost(uid, username, title, body) {
        // A post entry.
        var postData = {
            author: username,
            uid: uid,
            body: body,
            title: title,
            starCount: 0,
        };

        return firebase.database().ref().child('posts').push(postData);
    }

~~~SECTION:notes~~~
returns ThenableReference Combined Promise and reference; resolves when write is complete, but can be used immediately as the reference to the child location.
~~~ENDSECTION~~~


!SLIDE code
# Database Writing, Update (web)

    @@@ Javascript
    function writeNewPost(uid, username, title, body) {
        // A post entry.
        var postData = {
            author: username,
            uid: uid,
            body: body,
            title: title,
            starCount: 0,
        };

        // Get a new key
        var newPostKey = firebase.database().ref().child('posts').push().key;

        // Write data simultaneously in posts list and user's post list.
        var updates = {};
        updates['/posts/' + newPostKey] = postData;
        updates['/user-posts/' + uid + '/' + newPostKey] = postData;

        return firebase.database().ref().update(updates);
    }


~~~SECTION:notes~~~
Using these paths, you can perform simultaneous updates to multiple locations in the JSON tree with a single call to update(), such as how this example creates the new post in both locations. Simultaneous updates made this way are atomic: either all updates succeed or all updates fail.
~~~ENDSECTION~~~

!SLIDE code
# Database Writing, Transaction (web)

    @@@ Javascript
    function toggleStar(postRef, uid) {
      // Create a transaction to update multiple places at once
      return postRef.transaction(function(post) {
          if (post) {
              if (post.stars && post.stars[uid]) {
                  post.starCount--;
                  post.stars[uid] = null;
              } else {
                  post.starCount++;
                  if (!post.stars) {
                      post.stars = {};
                  }
                  post.stars[uid] = true;
              }
          }
          return post;
        });
    }

~~~SECTION:notes~~~
Using a transaction prevents star counts from being incorrect if multiple users star the same post at the same time or the client had stale data. If the transaction is rejected, the server returns the current value to the client, which runs the transaction again with the updated value. This repeats until the transaction is accepted or you abort the transaction.

Returns a Promise containing {committed: boolean, snapshot: nullable firebase.database.DataSnapshot}
~~~ENDSECTION~~~

!SLIDE code
# Database Remove (web)

    @@@ Javascript
    var adaRef = firebase.database().ref('users/ada');
    adaRef.remove()
        .then(function() {
            console.log("Remove succeeded.")
        })
        .catch(function(error) {
            console.log("Remove failed: " + error.message)
        });

~~~SECTION:notes~~~
Returns Promise containing void, Resolves when remove on server is complete.~~~ENDSECTION~~~

