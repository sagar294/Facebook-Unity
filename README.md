# Facebook-Unity

# Initialized
    if (!FB.IsInitialized)
    {
        FB.Init(() =>
        {
            if (FB.IsInitialized)
                FB.ActivateApp();
            else
                Debug.LogError("Couldn't initialize");
        },
        isGameShown =>
        {
            if (!isGameShown)
                Time.timeScale = 0;
            else
                Time.timeScale = 1;
        });
    }
    else
        FB.ActivateApp();

## Login
    var permissions = new List<string>() { "public_profile", "email", "user_friends" };
    FB.LogInWithReadPermissions(permissions, AuthCallback);

## Callback
    if (FB.IsLoggedIn)
    {
        var aToken = Facebook.Unity.AccessToken.CurrentAccessToken;
        Debug.Log(aToken.UserId);
        foreach (string perm in aToken.Permissions)
        {
            Debug.Log(perm);
            FriendsText.text += perm + "    ";
        }
    }
    else
    {
        FriendsText.text = "User cancelled login";
        Debug.Log("User cancelled login");
    }

## Logout
    FB.LogOut();
    
## Share
    FB.ShareLink(new System.Uri("https://resocoder.com"), "Check it out!",
      "Good programming tutorials lol!",
      new System.Uri("https://resocoder.com/wp-content/uploads/2017/01/logoRound512.png"));

## Facebook Game Request
    FB.AppRequest("Hey! Come and play this awesome game!", title: "Reso Coder Tutorial");
    
## Find Friends
    string query = "/me/friends";
    FB.API(query, HttpMethod.GET, result =>
    {
        var dictionary = (Dictionary<string, object>)Facebook.MiniJSON.Json.Deserialize(result.RawResult);
        var friendsList = (List<object>)dictionary["data"];
        FriendsText.text = "" + friendsList.Count;
        //foreach (var dict in friendsList)
            //FriendsText.text += ((Dictionary<string, object>)dict)["name"];
    });
