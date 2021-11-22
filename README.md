# Do-Re-Me

## Table of Contents
1. [Overview](#Overview)
2. [Product Spec](#Product-Spec)
3. [Wireframes](#Wireframes)

## Overview
### Description
Do-Re-Mi is a music app that allows people to discover new music based on location. Users can "drop" music at their location, and when other users walk by, they can add the song to their queue!

### App Evaluation
- **Category:** Social Networking / Music
- **Mobile:** This app would be primarily developed for mobile. Since users must get music based on their location, it will be impractical to develop features for a web application.
- **Story:** A user walks by a mall, and decides to drop their music at that location using Do-Re-Me. Their song is discoverable by other users for a few hours. Other users at the mall can add the song to their queue and listen to it.
- **Market:** Any individual could choose to use this app. It will likely see higher usage in population-dense places, such as a mall or a college-campus.
- **Habit:** This app could be used as often as users want. We believe users who walk, jog, or run past specific places may use this app often. Also,those who want to discover music can simply walk to a specific location and discover the music that people want to share.
- **Scope:** First, we will start with developing the feature to "drop" music at the user's current location. Then, we must ensure that the song can persist in that location for a few hours and is visible to all other users nearby. Then, we will develop the feature to add the specific song to a queue. Initially, we will use the Spotify API, but there is a large potential for use with multiple streaming service applications, such as YouTube, Apple Music, and Soundcloud. Furthermore, there is potential to develop a social networking and messaging feature, where users can view other users' profile and view the most current songs they've "dropped".

## Product Spec
### 1. User Stories (Required and Optional)

**Required Must-have Stories**

[X] User logs in with their username and password.
[X] User can see their current location and other spots where songs are dropped.
[] Users can listen to music at dropped sight and add to playlist if they like it. (Spotify API)
[] Profile pages for each user
[] Settings (Accesibility, Notification, General, etc.)

**Optional Nice-to-have Stories**

* Log of past songs/people with album art covers 
* Profile Add-On: Top music choices, etc.
* Optional Shuffle Button (i.e. random encounter/random song)
* Listening/Encounter Queue

### 2. Screen Archetypes

* Login 
* Register - User signs up or logs into their account
   * Upon Download/Reopening of the application, the user is prompted to log in to gain access to their profile information to be properly matched with another person. 
   * ...
* Messaging Screen - Chat for users to communicate (direct 1-on-1)
   * Upon selecting music choice users matched and message screen opens
* Profile Screen 
   * Allows user to upload a photo and fill in information that is interesting to them and others
* Song Selection Screen.
   * Allows user to be able to choose their desired song, artist, genre of preference and begin listening and interacting with others.
* Settings Screen
   * Lets people change language, and app notification settings.

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Music selection
* Profile
* Settings

Optional:
* Music/Encounter Queue
* Discover (Top Choices)

**Flow Navigation** (Screen to Screen)
* Forced Log-in -> Account creation if no log in is available
* Music Selection (Or Queue if Optional) -> Jumps to Chat
* Profile -> Text field to be modified. 
* Settings -> Toggle settings

## Wireframes
![Wireframe](Wireframe.png)

## Schema 
### Models
#### User

   | Property      | Type     | Description |
   | ------------- | -------- | ------------|
   | userId        | String   | unique id for the user (default field) |
   | username      | String   | username specified by client |
   | email         | String   | Email of user|
   | hash          | String   | encrypted password of the user |
   | salt          | String   | salt used for password encryption |
   | createdAt     | Date     | date when user account is created (default field) |
   | updatedAt     | Date     | date when user credentials were last updated (default field) |
   
#### Song Drop
   | Property      | Type     | Description |
   | ------------- | -------- | ------------|
   | objectID      | String   | unique id for the song drop (default field) |
   | user          | Pointer to the Poster   | Information on who posted the song|
   | songURI       | String   | The URI created by Spotify for its songs|
   | createdAt     | DateTime | date when the drop is created|
   | location      | [Latitude, Longitude] | The location where the song was dropped, and where the song will display on the map |

### Networking
* Dropping Songs (Create/POST)
``` swift
let url = NSURL(string: "Firebase URL Here")
guard let requestURL = url else { fatalError() }

var request = URLRequest(url: requestURL)
request.httpMethod = "POST"

let postString = "objectID=001&songURI=100&title=Industry Baby&user=123456&date=08/04/2001&pictureURL=URL&location=29.6499278,-82.3377014"

request.httpBody = postString.data(using: String.Encoding.utf8);

let task = URLSession.shared.dataTask(with: request) { (data, response, error) in
        
        // Check for Error 
        if let error = error {
            print("Error took place \(error)")
            return
        }
 
        // Convert HTTP Response Data to a String
        if let data = data, let dataString = String(data: data, encoding: .utf8) {
            print("Response data string:\n \(dataString)")
        }
}
task.resume()
```

* Getting Songs (Read/GET)
```swift
let url = URL(string: "Database URL")
guard let requestUrl = url else { fatalError() }

var request = URLRequest(url: requestUrl)
request.httpMethod = "GET"

let task = URLSession.shared.dataTask(with: request) { (data, response, error) in
    
    if let error = error {
        print("Error took place \(error)")
        return
    }
    
    if let response = response as? HTTPURLResponse {
        print("Response HTTP Status code: \(response.statusCode)")
    }
    
    // Convert HTTP Response Data to a simple String 
    if let data = data, let dataString = String(data: data, encoding: .utf8) {
        print("Response data string:\n \(dataString)")
    }
    
}
task.resume()
```
### Sprint 1

Very few lines of coded were needed for Sprint 1. Our Sprint 1 involved linking our xcode project with spotify and google firebase. To link with spotify we had to import the SpotifyiOS.framework, configure info.plst, Set the -ObjC Linker Flag, and Add a bridging error. To link with google firebase I had to import firebase, add the GoogleService-info.plst. Firebase also had to be imported through the pod file.

<img src='https://imgur.com/g6BQHJg.gif' title='Video Walkthrough' width='' alt='Video Walkthrough' />


