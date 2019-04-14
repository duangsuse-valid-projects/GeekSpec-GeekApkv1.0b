#### _Entity Data_ app

* id, Long
  * Generated, IDENTITY
* version, Long
  * Version

* packageName, String
  * Default `"(unset)"`
  * Validated `PackageName`
  * Size Validation `[0, 64)`: `"unexpected package name size, expected size in [0, 64)"`
  * Unique

  Package name for the application, unique in the table

* author, __User__

  User owning this application

* category, __Category__

  Category which this application is collected to

* name, String
  * Default `"App"`
  * Unicode Size Validation `(0, 100)`: `"expected application name grapheme cluster size to be in (0, 100)"`
  * Max Byte Size Validation `2K`
  * Natural Language

  Simple name may containing searchable AppAttributes `(attr=value)`

* readme, String
  * Default `""`
  * Markdown
  * Max Byte Size Validation `20K`
  * Large Object
  * Natural Language
  * Unicode Size Validation `(0, 15000)`: `"expected readme text size to be in (0, 15000)"`

  Markdown readme text

* icon, String
  * Default `"https://github.com/feathericons/feather/blob/7b8ff33195a5c3aacf6459bfc290558dad462285/icons/package.svg"`
  * URL
  * Large Object
  * Max Byte Size Validation `500K`

  Application icon

* screenshots, String
  * Default `"(hasShots=false)(reasonText=:()"`
  * Large Object
  * Size Validation `..1000`

  Application screenshot URLs text may containing AppType `(type=typeName)` and other attributes which are not searchable, links are splited by `";"`

* pinned, __Comment__?
  * Default `null`

  Applications can have one pinned-to-top comment handled by clients

* createdAt, Date
  * TimeStamp
  * Immutable
  * Default `Date()`

  Creation date

* updatedAt, Date
  * TimeStamp
  * Default `Date()`

  Last updated (published pairing reversion or metadata)

* collaborators, __User__*

  Application collaborators

* stargazers, __User__*

> _Weak fields_

* latestReversion, Int
  * Default `1`

  Last reversion

* starsCount, Int
  * Counts "stargazer"

* commentsCount, Int
  * Counts __Comment__

#### _Entity Relation_ app

* _Belonging to_ __User__ by __author__
  * _observes update_ `onOwnerChanged`
  * _observes delete_ `onOwnerUserDeletedNotify`
  * _on delete cascade_

* _Has one_ __Category__ by __category__

* _Has one_ __Comment__ by __pinned__
  * _observes update_ `onPinnedMessageChanged`
  * _on delete set_ `null`

* _Has many_ __Comment__ by __Comment.app__
  * _observes attach_ `onCommentAttached`

* _Has many_ __AppUpdate__ by __AppUpdate.forApp__
  * _observes attach_ `onNewReversion`

* _Has many_ __User__ "collaborator" by __collaborators__
  * _observes attach_ `onNewCollaborator`
  * _observes detach_ `onCollaboratorLeave`

* _Has many_ __User__ "stargazer" by __stargazers__
  * _observes attach_ `onNewStar`
