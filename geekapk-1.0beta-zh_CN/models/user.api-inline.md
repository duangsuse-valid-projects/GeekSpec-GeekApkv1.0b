#### _Entity Data_ user

* id, Int
  * Generated, IDENTITY

* username, String
  * Default `"(generated)"`
  * Size Validation `[1, 20)`: `"username size must be greater than 1 char and smaller than 21 char count"`
  * Natural Language
  * Validated `SimpleName`

  User's identical machine name

* nickname, String
  * Default `"Geek"`
  * Natural Language
  * Size Validation `.. 90`: `"nickname char size too long (> 90)"`
  * Unicode Size Validation `20`: `"nickname unicode size too long (> 20)"`

  User's display name

* avatarUrl, String
  * Default `"https://user-images.githubusercontent.com/10570123/52161551-8321ed00-2701-11e9-963b-18e5791d553c.png"`
  * URL
  * Large Object
  * Max Byte Size Validation `700K`

  User's avatar bitmap image link

* metaApp, __App__?
  * Default `null`

  User's meta application, may null

* bio, String
  * Markdown
  * Natural Language
  * Large Object
  * Default `"_No bio provided QAQ_"`
  * Max Byte Size Validation `10K`
  * Unicode Size Validation `.. 6000`: `"bio too long (~ ..6k)"`

* flags, UserStatus
  * Default `UserStatus.NONE`

  User status flag enumeration

* lastOnline, Date
  * TimeStamp
  * Default `Date()`

  A user can simply not to update online stat if he/she wants to be "invisible"

  GeekApk Stream in 2.0 will give real-time online stat

* sharedHash, String
  * Default `makeSharedHash(20)`
  * Protected

  "Distribute" Password

* hash, String
  * Default `"68e656b251e67e8358bef8483ab0d51c6619f3e7a1a9f0e75838d41ff368f728"`
  * Size Validation `64`
  * Protected

  Password for the user, sha256 hash hexdigest string

* createdAt, Date
  * TimeStamp
  * Default `Date()`
  * Immutable

  User's registration date

> _Weak fields_

* followersCount, Int
  * Countes "follower"

#### _Entity Relation_ user

* _Has many_ __App__ by __App.author__

* _Has many_ __App__ "stared" by set __App.stargazers__

* _Has many_ __App__ "collab" by __App.collaborators__

* _Has many_ __Comment__ by __Comment.author__

* _Has many_ __Notification__ by __Notification.owner__

* _Has many_ __Timeline__ by __Timeline.owner__
