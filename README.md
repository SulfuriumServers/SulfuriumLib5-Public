# SulfuriumLib5-Public
Public utility classes from SulfuriumLib5

[![GitHub license](https://img.shields.io/github/license/SulfuriumServers/SulfuriumLib5-Public.svg)](https://github.com/SulfuriumServers/SulfuriumLib5-Public/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/SulfuriumServers/SulfuriumLib5-Public.svg)](https://GitHub.com/SulfuriumServers/SulfuriumLib5-Public/releases/)
[![GitHub stars](https://img.shields.io/github/stars/SulfuriumServers/SulfuriumLib5-Public.svg)](https://GitHub.com/SulfuriumServers/SulfuriumLib5-Public/stargazers/)

This is the newest version of the SulfuriumLib plugin. It introduces many new APIs targeted to make developement easier and bundle important tasks.

## ItemFactory

Create custom items & armor using a few lines of code having a per-user translation to make localisation none of your business and reduce language barriers. Its like ItemsAdder but with less and more important features. Its allows for persistent custom Items as well as custom enchantments.

```java
ItemFactory factory = new ItemFactory(true); // Register vanilla items
ItemParityListener.setup(factory); // Setups a packet listener to enable localisation

CustomItemMeta customItem = new DefaultItemInfo.ARMOR.Builder(){{
    identifier = "sulfuriumadventure:void_chestplate"; // Persistent identifier for the item
    rarity = CustomItemRarity.MYTHIC; // Only cosmetic
    slot = EquipmentSlot.CHEST; // Determines the armor slot & item
    customModelData = 11000;
    color = ColorUtils.colorFromHex("#ad0057"); // For the use of FancyPants
    health = 100;
    defense = 50; // Only the needed stats are supplied
}}.build();

factory.getRegistry().registerItem(customItem);

ItemStack stack = factory.createItem(customItem); // Create the custom item base stack
factory.apply(customItem, true); // Applies all needed lore, etc. in format easily serializable by the packet listener and overrides all checks if already applied
```

## Texts

The Texts API provides a simplke way to add localisation. You can directly include .yml files at runtime to be included. Every User object has a Language field that can be used to translate the text.

```java
Texts.reload(); // Initializes the Texts API and loads library translations
System.out.println(Texts.trueFormat("%s %s!", "Hello", "World")); // Prints "Hello World!. Like string formating but only with %s so it doesnt interfere with other formating"
System.out.println(Texts.splitText("Lorem Ipsum [...] dolor sit amet", 32)); // Splits the text into multiple lines with a max length of 32 characters

Texts.getRawKey("message.join.game", Language.EN_US); // Returns the raw content of the key. Returns the key if not found
Texts.getRawKey("message.join.game", Language.EN_US, "PlayerName"); // Returns the raw content of the key. Returns the key if not found. Applies formating

Texts.getKey("message.join.game", Language.EN_US); // Returns a component serrialized with MiniMessage from the keys value.
Texts.getKey("message.join.game", Language.EN_US, "PlayerName");  // Applies formating

//TODO: getLore
//TODO: getKeyAsList

Texts.inject(Language.EN_US, MyPlugin.INSTANCE, "path/to/file.yml"); // Injects a file into the Texts API
```

## Logger

```java
Logger logger = new Logger("MyPluginName", NamedTextColor.GOLD);
Logger logger2 = new Logger(MyPlugin.INSTANCE, "MyPluginName", NamedTextColor.GOLD); // Creates a special logger for consoles that dont support console coloring

logger.__base(Logger.Type.LOG, "My message")a
logger.log("My message") // The same in a more human readable format
```

## Nametags

The library provides a simple API to create custom nametags per player.

```java
User user1 = MyPlugin.getUserRegistry().getUser(UUID.fromString("..."));
User user2 = MyPlugin.getUserRegistry().getUser(UUID.fromString("..."));

//TODO: Add example
```

## GUI

The library provides a simple API to create custom GUIs per player. This includes chest and anvil interfaces, as well as scoreboard and bossbar display builders.

```java
User user = MyPlugin.getUserRegistry().getOnlinePlayers().get(0);
GUIBUilder builder = user.getGUIBuilder();
builder.chestGUI(6); // Creates a 6x9 GUI

builder.setSlot(0, new ItemBuilder(Material.DIAMOND).build(), (usr) -> {
    usr.sendMessage("Clicked on slot 0");
});

builder.setSlot(1, new ItemBuilder(Material.DIAMOND).build(), null); // No action
builder.setSlot(2, 0, new ItemBuilder(Material.DIAMOND).build(), null); // Slot access using xy coordinates instad of index
```

## Guilds & Friends

The library handles a lot of different background tasks that do not directly expose interesting interfaces, such as guilds. These include invites, etc.

## Contributors

- [FabulousCodingFox (Fabian F.)](https://fabulouscodingfox.github.io/ "Website") - Head Developer
