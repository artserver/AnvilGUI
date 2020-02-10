# Jar 빌드 버전
다음에 사용할 때 머리가 덜 아프도록 플러그인으로 빌드해 두었습니다.\
Jar 위치는 아실 것이라고 믿고, 사용법도 아시리라 믿습니다.\
단, 이 플러그인이 실제로 하는 짓은 없습니다.

앞으로 Commit 할 일은 없을 것 같으니
Jar만 받아서 사용해주세요.

# AnvilGUI
Easily use anvil guis to get a user's input.

This project was made since there is no way to prompt users with an anvil input with the Spigot / Bukkit API. It requires interaction with NMS and that is a pain in plugins where users have different versions of the server running.

## Requirements
Java 8 and Bukkit / Spigot. Most server versions in the [Spigot Repository](https://hub.spigotmc.org/nexus/) are supported.

### My version isn't supported
If you are a developer, submit a pull request adding a wrapper class for your version. Otherwise, please create an issue
on the issues tab. 

## How to use

AnvilGUI requires the usage of Maven or a Maven compatible build system. 
You can find the repository and dependency information [here](https://jitpack.io/#WesJD/AnvilGUI).

### In your plugin

The `AnvilGUI.Builder` class is how you build an AnvilGUI. 
The following methods allow you to modify various parts of the displayed GUI. Javadocs are available [here](http://docs.wesjd.net/AnvilGUI/).

#### `onClose(Consumer<Player>)` 
Takes a `Consumer<Player>` argument that is called when a player closes the anvil gui.
```java                                             
builder.onClose(player -> {                         
    player.sendMessage("You closed the inventory.");
});                                                 
``` 

#### `onComplete(BiFunction<Player, String, AnvilGUI.Response>)`  
Takes a `BiFunction<Player, String, AnvilGUI.Response>` argument. The BiFunction is called when a player clicks the output slot. 
The supplied string is what the player has inputted in the renaming field of the anvil gui. You must return an AnvilGUI.Response,
which can either be `close()` or `text(String)`. Returning `close()` will close the inventory; returning `text(String)` will keep
the inventory open and put the supplied String in the renaming field.           
```java                                                
builder.onComplete((player, text) -> {                 
    if(text.equalsIgnoreCase("you")) {                 
        player.sendMessage("You have magical powers!");
        return AnvilGUI.Response.close();              
    } else {                                           
        return AnvilGUI.Response.text("Incorrect.");   
    }                                                  
});                                                    
```                                                    

#### `preventClose()` 
Tells the AnvilGUI to prevent the user from pressing escape to close the inventory.
Useful for situations like password input to play.                                      
```java                     
builder.preventClose();     
```                         
     
#### `text(String)`
Takes a `String` that contains what the initial text in the renaming field should be set to.
```java                                           
builder.text("What is the meaning of life?");     
```  

#### `item(ItemStack)`
Takes a custom `ItemStack` to be placed in the input slot.
```java                                              
ItemStack stack = new ItemStack(Material.GOLD_BLOCK);
ItemMeta meta = stack.getItemMeta();                 
meta.setLore(Arrays.asList("Hi there"));             
stack.setItemMeta(meta); 
builder.item(stack);        
```                                                  

#### `title(String)`
Takes a `String` that will be used as the inventory title. Only displayed in Minecraft 1.14 and above.
```java                            
builder.title("Enter your answer");
```                                
                 
#### `plugin(Plugin)`
Takes the `Plugin` object that is making this anvil gui. It is needed to register listeners.
```java                                         
builder.plugin(pluginInstance);                 
```                            

#### `open(Player)`
Takes a `Player` that the anvil gui should be opened for. This method can be called multiple times without needing to create
a new `AnvilGUI.Builder` object.                                                                                            
```java              
builder.open(player);
```                  

### A full example combining all methods
```java
new AnvilGUI.Builder()
    .onClose(player -> {                      //called when the inventory is closing
        player.sendMessage("You closed the inventory.");
    })
    .onComplete((player, text) -> {           //called when the inventory output slot is clicked
        if(text.equalsIgnoreCase("you")) {
            player.sendMessage("You have magical powers!");
            return AnvilGUI.Response.close();
        } else {
            return AnvilGUI.Response.text("Incorrect.");
        }
    })
    .preventClose()                           //prevents the inventory from being closed
    .text("What is the meaning of life?")     //sets the text the GUI should start with
    .item(new ItemStack(Material.GOLD_BLOCK)) //use a custom item for the first slot
    .title("Enter your answer.")              //set the title of the GUI (only works in 1.14+)
    .plugin(myPluginInstance)                 //set the plugin instance
    .open(myPlayer);                          //opens the GUI for the player provided
```
                                                                                                                                                                                                                                                                              

## Compilation
Build with `mvn clean install`.

## License
This project is licensed under the [MIT License](LICENSE).
