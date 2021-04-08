<div align="center">
   <h1>Account Creation Process</h1>
   <a href="https://reactnative.gallery/xcarpentier/e0d8eff7-0dfb-4823-8576-a615267354cc">
    <img alt="react-native-redflag-account-creation" src="https://thumbs.gfycat.com/CautiousSpanishAnhinga-size_restricted.gif" width="260" height="510" />
   </a>
   <h1>Signing In</h1>
   <a href="https://reactnative.gallery/xcarpentier/e0d8eff7-0dfb-4823-8576-a615267354cc">
    <img alt="react-native-redflag-account-creation" src="https://thumbs.gfycat.com/AngryCostlyAmphiuma-size_restricted.gif" width="260" height="510" />
   </a>
   <h1>Editing Account</h1>
   <a href="https://reactnative.gallery/xcarpentier/512c304b-00f4-4d9e-a9a7-94dca48e79dd">
    <img alt="react-native-redflag-account-editing" src="https://media4.giphy.com/media/oIhcy8vQveEXIs5ndc/giphy.gif" width="260" height="510" />
 </a>
   <h1>Viewing other account's</h1>
   <a href="https://reactnative.gallery/xcarpentier/512c304b-00f4-4d9e-a9a7-94dca48e79dd">
    <img alt="react-native-redflag-account-viewing" src="https://thumbs.gfycat.com/EnergeticQueasyHypsilophodon-size_restricted.gif" width="260" height="510" />
 </a>
   <h1>Matching with someone</h1>
   <a href="https://reactnative.gallery/xcarpentier/512c304b-00f4-4d9e-a9a7-94dca48e79dd">
    <img alt="react-native-redflag-account-viewing" src="https://thumbs.gfycat.com/OptimisticAmazingKoodoo-size_restricted.gif" width="260" height="510" />
 </a>
   <h1>Messaging and sending red flags</h1>
   <a href="https://reactnative.gallery/xcarpentier/512c304b-00f4-4d9e-a9a7-94dca48e79dd">
    <img alt="react-native-redflag-account-viewing" src="https://thumbs.gfycat.com/InsistentDefiniteAstrangiacoral-size_restricted.gif" width="260" height="510" />
 </a>
   <h1>Other user</h1>
   <a href="https://reactnative.gallery/xcarpentier/512c304b-00f4-4d9e-a9a7-94dca48e79dd">
    <img alt="react-native-redflag-account-viewing" src="https://thumbs.gfycat.com/HideousFittingAndeancat-size_restricted.gif" width="260" height="510" />
 </a>
</div>

**Front End**

Expo / React Native / TypeScript / Redux

bottomTabNavigator with React Navigation v5, Typescript tabs



*   Used color constants to ensure uniform look and feel across the entire app
*   Wrote custom Queries/Mutations/Subscriptions with GraphQL to make sure no extra data was pulled and scalability costs were cheap
*   Used Amazon Cognito to handle User Authentication as well as SMS OTPs, lambda functions for passwordless authentication since cognito doesn’t fully support it yet.
*   Additionally after Amazon Cognito verifies a user we store the cognito’s userId in DynamoDB
*   Expo push tokens stored in database allowing push notifications to users devices
*   Redux to handle User’s searching/dating preferences as well as Editing Profile View 
    *   Separate editing redux profile state for viewing purposes
    *   State management for all matches and chat rooms
    *   User preferences stored in state that connects to dynamo
*   Expo Image Picker and Amazon S3 Bucket for handling 6 Profile Images for dating profile
*   Custom Components for reusability across the app. (Showcasing user profile views, text inputs, image inputs, avatars etc.)
*   Header
    *   React native navigation V5
    *   Nested top tab navigators inside bottom tab navigator
*   Match screen uses latitude longitude geohashing, allowing us to grab users from our databases within X miles for an extremely inexpensive graphql filter solution
*   Match screen includes basic React Native Animations for smooth 60 fps transitions
*   Message Screen shows matches and chatrooms grabbed with graphql and live subscriptions listeners waiting for new matches/chatrooms and likes
*   Message Screen uses unique IDs for chatrooms to be created in pubnub
    *   Implemented PubNub API for live typing indicators, users presence, read/unread counts, live messages
    *   Pagination for grabbing old messages
    *   Took advantage of packages and used React Native Gifted Chat to hook up our backend message data for a more up to date messaging feel
*   RedFlag Matching Screen
    *   The bread and butter to our app, Users can send RedFlag secrets in real time to their matched user’s. They also can start a MUTUAL redflag share, 
        **   We updated our database to initiate a mutual share request with the other user and connection to chatroom
        **   The other user is notified of the mutual share and has the option of accepting or declining
*   Profile Screen
    *   View/Edit Screen
        **   User can do live edits to their profile and see exactly how it will look to other users on the dating app
        **   They can save the profile changes or delete when exiting the page
            *   Handled by useLayoutEffect
    *   Preference Screen
        **   User can update their preference for matching with other users
            *   Same School Filter
            *   Min/Max age Filter
            *   Distance Filter
            *   Gender Preference Filter
*   Recommendation Screen
    *   Grabs users in database based off of your user preferences that runs through an algorithm that’s based on interests
        **   If a users interests match more than 40% of current users interests show them on the recommendation page
*   LikesYou Screen
    *   Using GraphQL we pull users who like you and show them
*   Sentry.io built in to our react native app for error handling on the app store
*   Google Developer Account for Location API key for Schools and Cities
*   Incorporated React Native Type Form for beta users feedback

**Back End**

For our backend we used AWS Amplify/AppSync for a GraphQL API linked with DynamoDB


    Our tables in GraphQL included



*   User types to hold the users information for their 
    *   dating profile,
    *   location (longitude, latitude, and geohash),
    *   Last time the user opened the app and how much interactions they have done in the last 24 hours,
    *   Amazon S3 Key Images
    *   Pushtokens,
    *   ChatRooms,
    *   Matches,
    *   etc...
*   Match types
    *   This helped us handle when two users were matched and when a user viewed this match
    *   We left this as it’s own table for further features down the line instead of combining it with chat room tables
*   ChatRoom types
    *   This helped us handle users in chat rooms, using a few types we have the ability for 1v1 chat rooms or group chats
    *   We store information time tokens in the chat rooms to see when users have last been active in the chat room
    *   When the users first started the chatroom together
*   To gather a better understanding we hand built live chat and messages with subscription listeners. 
    *   We built Message types in graphql and had a connection to chatrooms for each message. 
    *   We then handled read messages and the unread message count in our backend
*   We then integrated PubNub Real Time in-app Chats instead of amazons solution for future development and bigger use cases
*   Custom subscription Listeners
    *   We have custom subscriptions listeners for real time in app experience
        **   Creating/Deleting/Updating Matches
        **   Creating/Deleting/Updating Chatrooms
*   We created algorithms that handled when matches needed to be created for the best real time experience as well as when chatrooms needed to be created. 
    *   We cleaned our databases based on this as well. Deleting information in DB when new chatrooms were created and old match data did not need to be used.
    *   Additionality we added a report system linking to a custom chatroom that we can see with the reported usersId, chatroomId and what they did… ‘sexual harassment etc…’ which we can verify by looking at that chatroomsId and ban them. 
