Redis
==================================

list: adlist.h

双向链表

dict: dict.h

dict -> 2*dictht -> dictEntry

ziplist

<zlbytes><zltail><zllen><entry><entry><zlend>

zipmap

<zmlen><len>"foo"<len><free>"bar"<len>"hello"<len><free>"world"
