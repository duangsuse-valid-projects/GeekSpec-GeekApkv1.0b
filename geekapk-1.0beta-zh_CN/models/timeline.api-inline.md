#### _Entity Data_ timeline

* id, Long
  * Generated, IDENTITY
* owner, __User__
  * Immutable

  Published by user

* type, TimelineType
  * Immutable
  * Default `TimelineType.FORWARD`

  Timeline type

* extra, Int
  * Immutable
  * Default `0`

  Paired data

* createdAt, Date
  * TimeStamp
  * Immutable
  * Default `Date()`

  Created date timestamp

#### _Entity Relation_ timeline

* _Belonging to_ __User__ by __owner__
  * _on delete cascade_
