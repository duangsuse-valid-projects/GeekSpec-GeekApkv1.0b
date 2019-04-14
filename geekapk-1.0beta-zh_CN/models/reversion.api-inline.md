#### _Entity Data_ update

* id, Long
  * Generated, IDENTITY

* forApp, __App__
  * Immutable

  Owning geekapk application

* reversion, Int
  * Default `1`
  * Unique by __forApp__

  Reversion id, unique to one application

* version, String
  * Default `"v1.0"`
  * Unicode Size Validation `[0, 64)`
  * Max Byte Size Validation `2K`

  Application version name

* updates, String
  * Default `"(none)"`
  * Markdown
  * Large Object
  * Natural Language
  * Unicode Size Validation `(0, 15000)`: `"expected update description text size to be in (0, 15000)"`
  * Max Byte Size Validation `15K`

  Updates description document

* minSdk, Int
  * Default `14`

  Minimal Android Runtime version required to parse the package

* installLink, String
  * Default `"no-source:to-be-filled"`
  * Large Object
  * Max Byte Size Validation `900K`

  GeekLink to perform install operation

* createdAt, Date
  * TimeStamp
  * Default `Date()`
  * Immutable

  Creation time for this update

#### _Entity Relation_ update

* _Belonging to_ __App__ by __forApp__
  * _on delete cascade_
