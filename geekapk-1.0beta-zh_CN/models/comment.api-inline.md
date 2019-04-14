#### _Entity Data_ comment

* id, Long
  * Generated, IDENTITY

* author, __User__
  * Immutable

  User whose published this comment

* app, __App__
  * Immutable

  Linked app (belonging to)

* replies, __Comment__?
  * Immutable

  Maybe replies to comment in the same app

* content, String
  * Default `""`
  * Large Object
  * Markdown
  * Natural Language
  * Max Byte Size Validation `6K`
  * Unicode Size Validation `.. 6000`: `"comment too long (at most 6k characters)"`

  It's content markdown text

* createdAt, Date
  * TimeStamp
  * Default `Date()`
  * Immutable

  Created at timestamp

* updatedAt, Date
  * TimeStamp
  * Default `Date()`

  Modified by owner at timestamp

> _Weak fields_

* repliesCount, Long
  * Counts "reply"

  Comment replies number

#### _Entity Relation_ comment

* _Belonging to_ __User__ by __author__
* _Belonging to_ __App__ by __app__
* _Belonging to_ __Comment__ by __replies__
* _Has many_ __Comment__ "reply" by __Comment.replies__
  * _observes attach_ `onNewReply`
