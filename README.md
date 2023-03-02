# RiverCrossingGame


Let's go through each method in the GameEngine interface one by one:

1. **`numberOfItems()`**: This method returns the number of items in the game.
2. **`getItemLabel(Item item)`**: This method returns the label (name) of the specified item.
3. **`getItemColor(Item item)`**: This method returns the color of the specified item.
4. **`getItemLocation(Item item)`**: This method returns the location of the specified item.
5. **`setItemLocation(Item item, Location location)`**: This method sets the location of the specified item to the specified location.
6. **`getBoatLocation()`**: This method returns the location of the boat.
7. **`loadBoat(Item item)`**: This method loads the specified item onto the boat.
8. **`unloadBoat()`**: This method unloads the items from the boat.
9. **`rowBoat()`**: This method moves the boat to the other side of the river.
10. **`gameIsWon()`**: This method returns true if the game is won, false otherwise.
11. **`gameIsLost()`**: This method returns true if the game is lost, false otherwise.
12. **`resetGame()`**: This method resets the game to its initial state.

To use this interface, you would need to create a class that implements it and provides an implementation for each of the methods. This class would then be the game engine for your RiverGUI game.


AbstractGameEngine class

An abstract class is a class that cannot be instantiated, meaning you cannot create an object of that class. Instead, you can only create objects of its subclasses. Abstract classes are used to provide a common interface for its subclasses and to define some common behavior that its subclasses can inherit. 

In this case, the “AbstractGameEngine” class is an abstract class that implements the “GameEngine” interface. It provides a partial implementation of the interface, leaving some of the methods to be implemented by its subclasses. Lets go through the class line by line

```java
package river;
import java.awt.*;
import java.util.HashMap;
import java.util.Map;
public abstract class AbstractGameEngine implements GameEngine {

    protected Map <Item, GameObject> itemMap = new HashMap<>();
    protected Location boatLocation = Location.START;
    protected int boatCapacity = 2;

    @Override
    abstract public int numberOfItems();

    @Override
    public String getItemLabel(Item item) {
        return itemMap.get(item).getLabel();
    }

    @Override
    public Color getItemColor(Item item) {
        return itemMap.get(item).getColor();
    }

    @Override
    public Location getItemLocaation(Item item)
    {
        return itemMap.get(item).setLocation(location);
    }

    @Override
    public Location getBoatLocation()
    {
        return boatLocation;
    }

    @Override
    public void loadBoat(Item item)
    {
        if (itemMap.get(item).getLocation() == boatLocation && boatCapacity > 0)
        {
            itemMap.get(item).setLocation(Location.BOAT);
            boatCapacity--;
        }
    }

    @Override
    public void unloadBoat(Item item)
    {
        if (itemMap.get(item).getLocation() == Location.BOAT)
        {
            itemMap.get(item).setLocation(boatLocation);
            boatCapacity++;
        }
    }

    @Override
    public void rowBoat()
    {
        assert (boatLocation != Location.BOAT);
        boatLocation = (boatLocation == Location.START) ? Location.FINISH : Location.START;
    }

    @Override
    public boolean gameIsWon()
    {
        for(Item item : Item.values())
        {
            if(!(item.ordinal() < numberOfItems())) break;
            if (getItemLocation(item) != Location.FINISH)
            {
                return false;
            }
        }

        return true;
    }

    @Override
    public boolean gameIsLost()
    {
        return false;
    }

    @Override
    public void resetGame()
    {
        for(Item item : Item.values())
        {
            if (!(item.ordinal() < numberOfItems())) break;
            itemMap.get(item).setLocation(Location.START);
        }

        boatLocation = Location.START;
        boatCapacity = 2;
    }
}
```

1. **`protected Map<Item, GameObject> itemMap = new HashMap<>();`**: This line declares a map that maps **`Item`** objects to **`GameObject`** objects. The **`protected`** access modifier means that this field can be accessed by subclasses of **`AbstractGameEngine`**.
2. **`protected Location boatLocation = Location.START;`**: This line declares a **`Location`** object representing the location of the boat. The initial value is **`Location.START`**. Again, this field is **`protected`** and can be accessed by subclasses.
3. **`protected int boatCapacity = 2;`**: This line declares an integer representing the capacity of the boat. The initial value is 2. Like the other fields, this field is **`protected`**.
4. **`@Override abstract public int numberOfItems();`**: This line is an abstract method that returns the number of items in the game. It is marked as **`abstract`**, which means that it must be implemented by any subclass of **`AbstractGameEngine`**.
5. **`@Override public String getItemLabel(Item item) { ... }`**: This line overrides the **`getItemLabel`** method defined in the **`GameEngine`** interface. It retrieves the label (name) of the specified item from the **`itemMap`**.
6. **`@Override public Color getItemColor(Item item) { ... }`**: This line overrides the **`getItemColor`** method defined in the **`GameEngine`** interface. It retrieves the color of the specified item from the **`itemMap`**.
7. **`@Override public Location getItemLocation(Item item) { ... }`**: This line overrides the **`getItemLocation`** method defined in the **`GameEngine`** interface. It retrieves the location of the specified item from the **`itemMap`**.
8. **`@Override public Location getBoatLocation() { ... }`**: This line overrides the **`getBoatLocation`** method defined in the **`GameEngine`** interface. It returns the location of the boat.
9. **`@Override public void loadBoat(Item item) { ... }`**: This line overrides the **`loadBoat`** method defined in the **`GameEngine`** interface. It loads the specified item onto the boat if it is at the same location as the boat and the boat has capacity.
10. **`@Override public void unloadBoat(Item item) { ... }`**: This line overrides the **`unloadBoat`** method defined in the **`GameEngine`** interface. It unloads the specified item from the boat if it is on the boat.
11. **`@Override public void rowBoat() { ... }`**: This line overrides the **`rowBoat`** method defined in the **`GameEngine`** interface. It moves the boat to the other side of the river.
12. **`@Override public boolean gameIsWon() { ... }`**: This line overrides the **`gameIsWon`** method defined in the **`GameEngine`** interface. It checks if all the items are on the other side of the river.
13. **`@Override public boolean gameIsLost() { ... }`**: This line overrides the **`gameIsLost`** method defined in the **`GameEngine`** interface. It

Item Enum

```java
package river;

public enum Item {
    ITEM_0, ITEM_1, ITEM_2, ITEM_3, ITEM_4, ITEM_5
}
```

The **`Item`** enum in the **`river`** package is used to represent the different items that need to be transported across the river in the RiverGUI game.

The enum defines six different constants, each representing one of the items in the game, using the syntax **`ITEM_n`**, where **`n`** is a number between 0 and 5. These constants are **`ITEM_0`**, **`ITEM_1`**, **`ITEM_2`**, **`ITEM_3`**, **`ITEM_4`**, and **`ITEM_5`**.

Enums are a special type in Java that allow you to define a fixed set of named values. In this case, the **`Item`** enum is being used to represent the different items in the game because it provides a convenient and type-safe way to refer to each item by name. For example, instead of using a String to represent an item name, you can use the **`Item`** enum constant **`ITEM_0`**.

Location Enum

```java
package river;

public enum Location
{
    START, FINISH, BOAT
}
```

The **`Location`** enum in the **`river`** package is used to represent the different locations in the RiverGUI game.

The enum defines three different constants, each representing one of the locations in the game, using the syntax **`LOCATION_NAME`**. These constants are **`START`**, **`FINISH`**, and **`BOAT`**.

Enums are a special type in Java that allow you to define a fixed set of named values. In this case, the **`Location`** enum is being used to represent the different locations in the game because it provides a convenient and type-safe way to refer to each location by name. For example, instead of using a String to represent a location name, you can use the **`Location`** enum constant **`START`**.

************************FarmerEngine************************

```java
package river;

import java.awt.*;
import java.util.HashMap;

public class FarmerGameEngine extends AbstractGameEngine {

    public static final Item BEANS = Item.ITEM_0;
    public static final Item GOOSE = Item.ITEM_1;
    public static final Item WOLF = Item.ITEM_2;
    public static final Item FARMER = Item.ITEM_3;

    public FarmerGameEngine() {
        itemMap = new HashMap() {{
            put(BEANS, new GameObject("B", Location.START, Color.CYAN));
            put(GOOSE, new GameObject("G", Location.START, Color.CYAN));
            put(WOLF, new GameObject("W", Location.START, Color.CYAN));
            put(FARMER, new GameObject("", Location.START, Color.MAGENTA));
        }};
    }

    @Override
    public int numberOfItems() {
        return itemMap.size();
    }

    @Override
    public void rowBoat() {
        assert (boatLocation != Location.BOAT);
        if (boatLocation == Location.START && getItemLocation(FARMER) == Location.BOAT) {
            boatLocation = Location.FINISH;
        } else if (boatLocation == Location.FINISH && getItemLocation(FARMER) == Location.BOAT) {
            boatLocation = Location.START;
        }
    }

    @Override
    public boolean gameIsLost() {
        if (getItemLocation(GOOSE) == Location.BOAT || getItemLocation(GOOSE) == getItemLocation(FARMER)
                || getItemLocation(GOOSE) == boatLocation) {
            return false;
        }
        return getItemLocation(GOOSE) == getItemLocation(WOLF) || getItemLocation(GOOSE) == getItemLocation(BEANS);
    }
}
```

The **`FarmerGameEngine`** class in the **`river`** package extends the **`AbstractGameEngine`** class and provides an implementation specific to the Farmer puzzle in the RiverGUI game.

The class defines four **`Item`** constants: **`BEANS`**, **`GOOSE`**, **`WOLF`**, and **`FARMER`**. These constants are used to identify the different game objects in the Farmer puzzle.

The class constructor initializes the **`itemMap`** HashMap with four GameObjects: one for each of the **`BEANS`**, **`GOOSE`**, **`WOLF`**, and **`FARMER`** items. The **`GameObject`** constructor takes a label, a location, and a color as arguments. The label is a string that identifies the object in the game, the location is the object's starting location, and the color is used to draw the object on the game board.

The **`numberOfItems()`** method is implemented to return the size of the **`itemMap`** HashMap.

The **`rowBoat()`** method is overridden to update the **`boatLocation`** variable when the boat is moved. If the boat is currently at the start location and the farmer is on the boat, the boat is moved to the finish location. If the boat is currently at the finish location and the farmer is on the boat, the boat is moved to the start location.

The **`gameIsLost()`** method is overridden to check if the game has been lost based on the current item locations. If the goose is on the boat, or if the goose is on the same side of the river as the farmer or the boat, the game has not been lost. If the goose is on the same side of the river as the wolf or the beans, the game has been lost. The method returns true if the game has been lost and false otherwise.

**************************MonsterEngine**************************

```java
package river;

import java.awt.*;
import java.util.HashMap;

public class MonsterGameEngine extends AbstractGameEngine {

    public static final Item MONSTER1 = Item.ITEM_0;
    public static final Item MUNCHKIN1 = Item.ITEM_1;
    public static final Item MONSTER2 = Item.ITEM_2;
    public static final Item MUNCHKIN2 = Item.ITEM_3;
    public static final Item MONSTER3 = Item.ITEM_4;
    public static final Item MUNCHKIN3 = Item.ITEM_5;

    public MonsterGameEngine() {
        itemMap = new HashMap() {{
            put(MONSTER1, new GameObject("Mo", Location.START, Color.blue));
            put(MUNCHKIN1, new GameObject("Mu", Location.START, Color.pink));
            put(MONSTER2, new GameObject("Mo", Location.START, Color.blue));
            put(MUNCHKIN2, new GameObject("Mu", Location.START, Color.pink));
            put(MONSTER3, new GameObject("Mo", Location.START, Color.blue));
            put(MUNCHKIN3, new GameObject("Mu", Location.START, Color.pink));
        }};
    }

    @Override
    public int numberOfItems() {
        return itemMap.size();
    }

    @Override
    public void rowBoat() {
        assert (boatLocation != Location.BOAT);
        for (Item item : Item.values()) {
            if (!(item.ordinal() < numberOfItems())) break;
            if(getItemLocation(item) == Location.BOAT) {
                if (boatLocation == Location.START) {
                    boatLocation = Location.FINISH;
                } else if (boatLocation == Location.FINISH) {
                    boatLocation = Location.START;
                }
                break;
            }
        }
    }

    @Override
    public boolean gameIsLost() {
        // The first element of each array represents the number of the item
        // at the start and the second element is the number at the finish.
        int[] numMonsters = {0,0};
        int[] numMunchkins = {0,0};
        int i = 0;
        // Count how many monsters and munchkins are at the start and the finish
        for (Item item : Item.values()) {
            if (!(item.ordinal() < numberOfItems())) break;
            if (i % 2 == 0) {
                // If the item is a monster
                checkItemLocation(item, numMonsters);
            } else {
                // Else the item is a munchkin
                checkItemLocation(item, numMunchkins);
            }
            i++;
        }
        // Now check if the game is lost
        if (numMunchkins[0] == 0) {
            // If there are no munchkins at the start
            return numMonsters[1] > numMunchkins[1];
        } else if (numMunchkins[1] == 0) {
            // If there are no munchkins at the finish
            return numMonsters[0] > numMunchkins[0];

        } else {
            // Else there are munchkins at the start and the finish
            return numMonsters[0] > numMunchkins[0] || numMonsters[1] > numMunchkins[1];
        }
    }

    private void checkItemLocation(Item item, int[] numItems) {
        if (getItemLocation(item) == Location.START) {
            // If the item is at the start
            numItems[0]++;
        } else if (getItemLocation(item) == Location.FINISH) {
            // Else if the item is at the finish
            numItems[1]++;
        } else {
            // Else the item is on the boat
            if (getBoatLocation() == Location.START) {
                // If the boat is at the start
                numItems[0]++;
            } else if (getBoatLocation() == Location.FINISH) {
                // Else if the boat is at the finish
                numItems[1]++;
            }
        }
    }
}
```

This is a Java class called MonsterGameEngine that extends the AbstractGameEngine class. The MonsterGameEngine class contains methods for playing a game involving monsters and munchkins crossing a river using a boat. The class has six static final items: MONSTER1, MUNCHKIN1, MONSTER2, MUNCHKIN2, MONSTER3, and MUNCHKIN3, each of which is an instance of the Item class.

The constructor of the MonsterGameEngine class initializes a HashMap called itemMap with the six items, each of which is associated with a GameObject that has a name (either "Mo" for monsters or "Mu" for munchkins), a location (which is initially set to Location.START), and a color (either blue for monsters or pink for munchkins).

The class implements three methods from the AbstractGameEngine class: numberOfItems(), rowBoat(), and gameIsLost(). The numberOfItems() method returns the size of the itemMap HashMap, the rowBoat() method moves the boat from one side of the river to the other, and the gameIsLost() method determines whether the game has been lost based on the number of monsters and munchkins on either side of the river.

The class also contains a private helper method called checkItemLocation() that is used by the gameIsLost() method to count the number of monsters and munchkins on either side of the river. This method takes an item and an integer array called numItems as input, and increments either the first or second element of the numItems array depending on whether the item is at the start or finish of the river, or on the boat.

****************RiverGUI****************

```java
package river;

import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.util.HashMap;
import java.util.Map;

/**
 * Graphical interface for the River application
 *
 * @author Gregory Kulczycki and Miles Spence
 */
public class RiverGUI extends JPanel implements MouseListener {

    // ==========================================================
    // Private Fields
    // ==========================================================

    private final Map<Item, Rectangle> itemRectangleMap = new HashMap() {{
        put(Item.ITEM_0, new Rectangle(20, 215, 50, 50));
        put(Item.ITEM_1, new Rectangle(80, 215, 50, 50));
        put(Item.ITEM_2, new Rectangle(20, 155, 50, 50));
        put(Item.ITEM_3, new Rectangle(80, 155, 50, 50));
        put(Item.ITEM_4, new Rectangle(140, 155, 50, 50));
        put(Item.ITEM_5, new Rectangle(140, 215, 50, 50));
    }};
    private final int[] itemXOffset = {0, 60, 0, 60, 0, 60}; // Each element corresponds to an item's x offset
    private final int[] itemYOffset = {60, 60, 0, 0, 120, 120}; // Each element corresponds to an item's y offset
    private final int[] boatXOffset = {0, 410}; // Used to add 0 when boat is at the start or 410 when at the finish
    private int numItemsPaintedOnBoat = 0;
    private Rectangle boatRectangle = new Rectangle(140, 275, 110, 50);
    private final Rectangle restartFarmerGameRect = new Rectangle(290, 120, 100, 30);
    private final Rectangle restartMonsterGameRect = new Rectangle(400, 120, 100, 30);
    private GameEngine engine; // Model
    private boolean gameIsOver = false;

    // ==========================================================
    // Constructor
    // ==========================================================

    public RiverGUI() {
        engine = new FarmerGameEngine();
        addMouseListener(this);
    }

    // ==========================================================
    // Paint Methods (View)
    // ==========================================================

    /**
     * Create the GUI and show it. For thread safety, this method should be invoked
     * from the event-dispatching thread.
     */
    private static void createAndShowGUI() {
        // Create and set up the window
        JFrame frame = new JFrame("RiverCrossing");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        // Create and set up the content pane
        RiverGUI newContentPane = new RiverGUI();
        newContentPane.setOpaque(true);
        frame.setContentPane(newContentPane);

        // Display the window
        frame.setSize(800, 600);
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        // Schedule a job for the event-dispatching thread:
        // creating and showing this application's GUI.
        javax.swing.SwingUtilities.invokeLater(RiverGUI::createAndShowGUI);
    }

    @Override
    public void paintComponent(Graphics g) {
        // Update every rectangle's location
        updateLocationsOfItemRectangles();
        updateLocationOfBoatRectangle();

        // Set the background color to gray
        g.setColor(Color.GRAY);
        g.fillRect(0, 0, this.getWidth(), this.getHeight());

        // Paint every rectangle
        for (Item item : Item.values()) {
            if (!(item.ordinal() < engine.numberOfItems())) break;
            paintRectangle(g, engine.getItemColor(item), engine.getItemLabel(item), itemRectangleMap.get(item));
        }
        paintRectangle(g, Color.ORANGE, "", boatRectangle);

        // Check if the game is over
        if (engine.gameIsLost()) {
            paintMessage("You Lost!", g);
            gameIsOver = true;
        } else if (engine.gameIsWon()) {
            paintMessage("You Won!", g);
            gameIsOver = true;
        }

        // If the game is over, then offer to restart the game
        if (gameIsOver) {
            g.setColor(Color.BLACK);
            // Paint farmer game restart button
            g.fillRect(restartFarmerGameRect.x - 3, restartFarmerGameRect.y - 3,
                    restartFarmerGameRect.width + (2 * 3), restartFarmerGameRect.height + (2 * 3));
            paintRectangle(g, Color.PINK, "Farmer", restartFarmerGameRect);
            // Paint monster game restart button
            g.fillRect(restartMonsterGameRect.x - 3, restartMonsterGameRect.y - 3,
                    restartMonsterGameRect.width + (2 * 3), restartMonsterGameRect.height + (2 * 3));
            paintRectangle(g, Color.PINK, "Monster", restartMonsterGameRect);
        }
    }

    private void updateLocationsOfItemRectangles() {
        for (Item item : Item.values()) {
            if (!(item.ordinal() < engine.numberOfItems())) break;
            if (engine.getItemLocation(item) == Location.START) {
                // If the item is at the start
                itemRectangleMap.put(item, new Rectangle(20 + itemXOffset[item.ordinal()],
                        155 + itemYOffset[item.ordinal()], 50, 50));
                /* Each statement like the above adds the minimum x and y values for all
                 * items to the corresponding offset of the specific item. For example, the
                 * third munchkin is in the bottom right corner of all the items. Therefore,
                 * its x value is 20 + itemXOffset[item.ordinal()] (which is equivalent to
                 * itemXOffset[5] or 60) and its y value is 155 + itemYOffset[item.ordinal()]
                 * (which is equivalent to itemYOffset[5] or 120). So, (80, 275).
                 */
            } else if (engine.getItemLocation(item) == Location.FINISH) {
                // Else if the item is at the finish
                itemRectangleMap.put(item, new Rectangle(670 + itemXOffset[item.ordinal()],
                        155 + itemYOffset[item.ordinal()], 50, 50));
            } else {
                // Else the item is on the boat
                itemRectangleMap.put(item, new Rectangle(140 + boatXOffset[engine.getBoatLocation().ordinal()]
                        + itemXOffset[numItemsPaintedOnBoat], 215, 50, 50));
                numItemsPaintedOnBoat++;
            }
        }
        numItemsPaintedOnBoat = 0;
    }

    private void updateLocationOfBoatRectangle() {
        boatRectangle = new Rectangle(140 + boatXOffset[engine.getBoatLocation().ordinal()],
                275, 110, 50);
    }

    private void paintRectangle(Graphics g, Color itemColor, String itemLabel, Rectangle rectangle) {
        // Paint the Rectangle
        g.setColor(itemColor);
        g.fillRect(rectangle.x, rectangle.y, rectangle.width, rectangle.height);

        // Write the item's label inside the rectangle
        g.setColor(Color.BLACK);
        int fontSize = (rectangle.height >= 40) ? 36 : 18;
        if (itemLabel.equals("Mu") || itemLabel.equals("Mo")) {
            // Shrink the font if it's a monster or munchkin
            fontSize = 24;
        }
        g.setFont(new Font("Verdana", Font.BOLD, fontSize));
        FontMetrics fm = g.getFontMetrics();
        g.drawString(itemLabel, rectangle.x + rectangle.width / 2 - fm.stringWidth(itemLabel) / 2,
                rectangle.y + rectangle.height / 2 + fontSize / 2 - 4);
    }

    public void paintMessage(String message, Graphics g) {
        g.setColor(Color.BLACK);
        g.setFont(new Font("Verdana", Font.BOLD, 36));
        FontMetrics fm = g.getFontMetrics();
        g.drawString(message, 400 - fm.stringWidth(message) / 2, 100);
    }

    // ==========================================================
    // MouseListener Methods (Controller)
    // ==========================================================

    @Override
    public void mouseClicked(MouseEvent e) {
        if (gameIsOver) {
            // The game is over (either lost or won)
            if (this.restartFarmerGameRect.contains(e.getPoint())) {
                // If the farmer game button is pressed
                engine = new FarmerGameEngine();
            } else if (this.restartMonsterGameRect.contains(e.getPoint())) {
                // Else if the monster game button is pressed
                engine = new MonsterGameEngine();
            }
            // Reset the game
            engine.resetGame();
            gameIsOver = false;
            repaint();
            return;
        }

        for (Item item : Item.values()) {
            if (!(item.ordinal() < engine.numberOfItems())) break;
            // Respond to the click of any item
            clickItem(item, e);
        }
        if (boatRectangle.contains(e.getPoint())) {
            // Respond to the click of the boat
            engine.rowBoat();
            // Reminder: it is not necessary to check if the boat is allowed
            // to be rowed because the game engines already check this
        }
        repaint();
    }

    // A couple private helper methods for the mouseClicked method
    private void clickItem(Item item, MouseEvent e) {
        if (itemRectangleMap.get(item).contains(e.getPoint())) {
            // If the item is clicked
            if (engine.getItemLocation(item) == Location.START) {
                // If the item is at the start
                if (getBoatCapacity() > 0) {
                    // If there's room on the boat
                    engine.loadBoat(item);
                }
            } else if (engine.getItemLocation(item) == Location.FINISH) {
                // Else if the item is at the finish
                if (getBoatCapacity() > 0) {
                    // If there's room on the boat
                    engine.loadBoat(item);
                }
            } else {
                // Else the item is on the boat and is always free to unload
                engine.unloadBoat(item);
            }
        }
    }

    private int getBoatCapacity() {
        int boatCapacity = 2;
        for (Item item : Item.values()) {
            if (!(item.ordinal() < engine.numberOfItems())) break;
            if (engine.getItemLocation(item) == Location.BOAT) {
                boatCapacity--;
            }
        }
        return boatCapacity;
    }

    // ----------------------------------------------------------
    // None of these methods will be used
    // ----------------------------------------------------------

    @Override
    public void mousePressed(MouseEvent e) {
        //
    }

    @Override
    public void mouseReleased(MouseEvent e) {
        //
    }

    @Override
    public void mouseEntered(MouseEvent e) {
        //
    }

    @Override
    public void mouseExited(MouseEvent e) {
        //
    }
}
```

his code is a Java implementation of a game called "River Crossing Puzzle". The game involves moving various objects, represented by rectangles in this implementation, from one side of a river to the other using a boat. The player must follow certain rules, such as not overloading the boat or leaving certain objects alone together.

The code defines a graphical user interface (GUI) for the game using Java Swing, with the main class **`RiverGUI`** inheriting from **`JPanel`** and implementing the **`MouseListener`** interface. The game logic is handled by the **`GameEngine`** interface, with two different implementations provided by the classes **`FarmerGameEngine`** and **`MonsterGameEngine`**.

The GUI consists of a gray background with various rectangles representing objects on either side of the river, and an orange rectangle representing the boat. The GUI is updated using the **`paintComponent`** method, which loops through all the objects and calls the **`paintRectangle`** method to draw each rectangle with its corresponding color, label, and location. The GUI also displays a message if the game is won or lost, and provides buttons to restart the game if the game is over.

The **`RiverGUI`** class contains several private fields, including a **`Map`** that maps each rectangle to its corresponding object, arrays that define the offset of each rectangle from the minimum x and y values, and a **`Rectangle`** that represents the location and size of the boat. The class also defines methods to update the location of each rectangle and the boat, and to handle mouse events such as clicking on a rectangle or a restart button.

Overall, the code is well-structured and easy to follow, with clear comments and descriptive variable names.

The RiverGUI class is the main class for the graphical interface of the River application. It extends JPanel and implements MouseListener. It contains private fields such as a map that maps items to their rectangle on the GUI, arrays that store x and y offsets for items and the boat, the number of items painted on the boat, a rectangle for the boat, rectangles for the restart buttons, the game engine, and a boolean value to indicate whether the game is over.

The constructor initializes the game engine and adds a mouse listener to the GUI. The paintComponent() method is responsible for painting the GUI. It updates the location of each rectangle, sets the background color, and paints every rectangle. It then checks if the game is over, and if it is, it paints restart buttons for each game type.

The updateLocationsOfItemRectangles() method is responsible for updating the location of each item rectangle based on its current location in the game engine. The updateLocationOfBoatRectangle() method is responsible for updating the location of the boat rectangle based on the current state of the game engine.

The createAndShowGUI() method is responsible for creating and showing the GUI. It creates a JFrame, sets the content pane to an instance of the RiverGUI class, and sets the size and visibility of the frame.

The main() method is responsible for scheduling the creation and showing of the GUI on the event-dispatching thread using the invokeLater() method.

Overall, the RiverGUI class is responsible for displaying the game engine's state visually and for allowing the player to interact with the game using the mouse.
