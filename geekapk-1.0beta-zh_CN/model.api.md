# GeekApk Data Models

## _Models_ 关系数据模型 ⭐️

Duo Relations:

* User `star` App
* User `collab` App
* User `follow` User

### App `apps` 应用程序

* GeekApk Application

* belonging to user
* linked with category

* has name, packageName(unique identifier in table), readme, icon, screenshots
* created time, updated time, latest reversion index

* and counters: star, comment

_See_ [app model](models/app.api-inline.md)

### AppUpdate `updates` 应用程序版本更新

* GeekApk Application reversion

* belonging to app

* reversion field is unique to forApp

* has unique *reversion*, version name, minimal SDK version, install link, created date fields

_See_ [app update model](models/reversion.api-inline.md)

### Comment `comments` 应用评论

* GeekApk application comments

_See_ [comments model](models/comment.api-inline.md)

### Category `categories` 应用程序分类

#### _Entity Data_ category

* id, Int
  * Generated, IDENTITY

* name, String
  * Default `"(none)"`
  * Natural Language
  * Size Validation `[0, 60)`: `"category name too long or empty"`

  Category name, maybe hierarchical using `'/'` character

#### _Entity Relation_ category

* _Observes creation_ `onCategoryCreated`

♥￣\\\_(ㇱ)_/￣~~✐~~ 👈 此模型没有任何关系哦~

### User `users` 极安用户

* GeekApk users

_See_ [users model](models/user.api-inline.md)

### _enum_ `UserStatus`

* NOBODY
* BANNED
* READONLY
* NONE
* ADMIN

### Notification `notifications` 用户通知

* GeekApk user private notification box

_See_ [notification model](models/notification.api-inline.md)

### _enum_ `NotificationType`

* NEW_REPLY = 0
* REPLY_DELETED = 1
* NEW_APP = 2
* APP_DELETED = 3
* APP_MOVED = 4
* SHASH_RENEWED = 5
* REHASH = 6
* READWRITE = 100
* READONLY = 101
* BANNED = 102
* UNBANNED = 103
* SET_USER = 104
* PROMOTED = 105
* SYSTEM = 1000

### Timeline `site_timeline` 用户时间线记录

* A user's public timeline

_See_ [timeline model](models/timeline.api-inline.md)

### _enum_ `TimelineType`

* NEW_FOLLOW = 0
* NEW_APP = 1
* NEW_APP_UPDATE = 2
* NEW_REPLY = 3
* DELETED_APP = 4
* DELETED_APP_UPDATE = 5
* DELETED_REPLY = 6
* UPDATED_BIO = 7
* UPDATED_NAME = 8
* UPDATED_USERNAME = 9
* COLLABORATE_APP = 10
* NEW_STAR = 11
* FORWARD = 1000
