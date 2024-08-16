The saving system is built around the `SaveAble` class, which handles the serialization and deserialization of game objects. During initialization, the `_setup_saveable` method is called to register properties automatically as save-able. 

Any properties defined before the call to `_setup_saveable` are automatically added as save-able, unless they start with an underscore (`_`). 

For example, when saving a player's city, properties like population and resources are tracked and saved. The system also ensures state consistency through hashing, allowing objects like units, cities, and technologies to be reliably restored across sessions.

The save-able mix-in can be found at `openciv/engine/saving.py:Saveable`

```
class SaveableObject(Saveable):
	def __init__(self, a: int, b: int):
		super().__init__()
		self.a: int = a
		self._setup_saveable()
		self.b: int = b
```

When being saved the only property saved will be `a`.
Other properties can be added via calling
`_add_saveable_property(self, prop: str) -> None` 
in the implementing class of the object using the name of your property as string.
