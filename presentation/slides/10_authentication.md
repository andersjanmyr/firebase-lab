!SLIDE code
# Authentication (web)

    @@@ Javascript
    // Register the sign-in redirect handler
    firebase.auth().getRedirectResult().then(function(result) {
        if (result.credential) {
            var token = result.credential.accessToken;
        }
        // The signed-in user info.
        var user = result.user;
      }).catch(function(error) {
        // Handle Errors here.
        var errorCode = error.code;
        var errorMessage = error.message;
      });

    // Start the sign-in with the Google provider, similar for others
    var provider = new firebase.auth.GoogleAuthProvider();
    firebase.auth().signInWithRedirect(provider);


!SLIDE bullets

# Authentication (IOS, Android)

* Register your application.
* Do the OAuth2 dance
  - (too long to describe, we'll do it in the lab)
* Use the token to authenticate with Firebase

