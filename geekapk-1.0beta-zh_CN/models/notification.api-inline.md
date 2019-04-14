#### _Entity Data_ notification

* id, Long
  * Generated, IDENTITY
* owner, __User__
  * Immutable

  Notification owner

* type, NotificationType
  * Immutable
  * Default `NotificationType.SYSTEM`

  Notification type

* extra, Int
  * Immutable
  * Default `0`

  Paired data

* createdAt, Date
  * TimeStamp
  * Immutable
  * Default `Date()`

  Created date timestamp

* pass, Boolean
  * Default `false`

  Marked as read?

#### _Entity Relation_ notification
