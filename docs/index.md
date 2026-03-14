# Home
This doc describes usage of actions and behaviors which are a part of CopperCube Narrative Kit.
## Actions
### NK - Register Interactables Manager
This is the core action. It registers all systems used to interact with game objects (interactables). It should be added to "Before first drawing do something" behavior attached to the root node of game's starting scene. For debug purposes it also could be attached to any other scene, which doesn't affect final result in any way, since it is implemented the way it is registered only once.

Properties:

* `playerNodeName {string}` - Name of the FPS camera node controlled by the player. This node should have the same name across all gameplay scenes.
* `staticCameraNodeName {string}` - Name of simple camera that is used during interaction. This node should have the same name acrossall   gameplay scenes.
* `crosshairNodeName {string}` - Name of 2d overlay node that represents crosshair. This node should have the same name across all gameplay scenes.
* `interactActionTextNodeName {string}` - Name of 2d overlay node that is used to show helper text in case player is looking on an interactable node (ex. "Press [E] to interact"). This node should have the same name across all gameplay scenes.
* `detectorDistance {float}` - Distance in CopperCube units, at which a player can interact with interactable nodes. If you'reusing     default scaling default 26.0 should work fine.
* `interactKey {int}` - Keycode of the keyboard key used as action key. Default is E (69);
* `startWithGameplayScene {boolean}` - Interactables Manager should only be active during "gameplay" scenes, i.e. general scene there   player explores the environment and interacts with it. In case scene there the Manager is initialized is "none-gameplay"scene (like  cutscene, puzzle scene, pause menu, game menu and so on) this should be unchecked. Also even if not unchecked this should not break  anything, but you will get some console error messages.

### NK - Delete interactable
This action is used to delete interactables. In case the node you are removing has "Register as interactable" behavior attached you should use this one instead of built-in "Delete a scene node" action. It will both unregister an interactable node and remove it from the scene.

### NK - Give back control
This action is used to switch from static camera to player camera. Basically, it makes player move and interact with environment again.

### NK - Play sound from node
This is a helper function that allows playing sound once from a sound node.

Properties:

* `soundNode {scenenode}` - Sound node to play sound from.

### NK - Shake node
It is used to shake any 3d mesh node.

Properties:

* `nodeToShake {scenenode}` - scene node to apply animation to.
* `axis {string}` - axis on which the node shakes. Could be x, y, or z.
* `amplitude {float}` - amplitude of shake animation in CopperCube units.
* `times {int}` - defines how many times the node should shake.
* `speed {float}` - defines speed of animation.

### NK - Shown and hide nodes
Used to bulk show and hide nodes. Usually you can use built-in action for that, but in some specific cases the built-in one will not work. I recommend you use this one instead no matter what. In case you don't need all 8 slots just leave the ones you don't need empty - that will not have any effect on the game.

* `nodeToHide0 {scenenode}` - node to hide
* `nodeToHide1 {scenenode}` - node to hide
* `nodeToHide2 {scenenode}` - node to hide
* `nodeToHide3 {scenenode}` - node to hide
* `nodeToShow0 {scenenode}` - node to show
* `nodeToShow1 {scenenode}` - node to show
* `nodeToShow2 {scenenode}` - node to show
* `nodeToShow3 {scenenode}` - node to show

### NK - Show dialog message
Used to show text to a selected node and automatically give player control after action key is pressed again. You can specify multiple messages by separating them with "|" character.

* `dialogText {string}` - text message to show. It is possible to show multiple messages by separating them with "|" character.
* `nodeToShow {scenenode}` - node to show the text message.
* `interactionKey {number}` - keycode of the button that user have to press to close the message and give back control to player.
* `actionOnComplete {action}` - action that will be executed after dialog is closed.
* `doNotGiveBackControl` - do not give back control after interaction key is pressed. Static camera remains active.

### NK - Switch to scene
Used to switch to another game scene. You have to use this one instead of built-in CopperCube action, otherwise your interactables will not be properly registered on the new scene.

* `sceneName {string}` - name of the scene you are going to switch.
* `pointerNodeName {string}` - name of the pointer node inside the new scene. Pointer node is used as a reference for player's position and rotation as player enters a new scene, i.e. player FPS camera will receive the same position and rotation.
* `isGameplayScene {boolean}` - uncheck this in case the scene you're changing to is not a "gameplay" scene.

##Behaviors

### NK - Execute once on scene switch
Used to execute action only once on the first frame after player enters the scene. Additionally could be configured to only be executed in case variable has a specific value. This is useful for puzzle scenes, so you can propagate puzzle success resolution to another scene by setting a specific variable value.

* `variableName {string}` - variable name to check.
* `variableValue {string}` - variable value to check. In case this is set the action will only be executed in case variable has a specified value.
* `actionToExecute {action}` - action to execute.

### NK - Register interactable
Used to register node as "interactable" inside interactables manager.

* `message {string}` - message that will be shown in case player looks to the interactable. If you don't need a message to be shown - just leave it blank. Node with name specified during executing "NK - Register Interactables Manager" will be used to show the message.
* `nodeToShow {scenenode}` - scene node (2 overlay node) to be shown. If you don't need any additional nodes to be shown you can leave that blank. This is added in case you need to add some sort of image like fx "hand icon" or "eye icon" along with or instead of the text message.
* `actionOnInteract {action}` - action that will be executed in case player presses the action key.
* `alternativeInteractKey {string}` - you can specify an alternative keycode of the key that will be used as interaction key. If blank default key specified during executing "NK - Register Interactables Manager" will be used. You can also use mouse buttons (0 for lmb and 1 for rmb).
* `overwriteDetectionDistance {string}` - overwrite default detection distance. Use it in case you want player to come closer/further to the intractable node in order to interact. Leave empty if you don't need to use it.
* `conditionalVarName {string}` - name of variable to check.
* `conditionalVarValue {string}` - value of variable to check. In case both conditionalVarName and conditionalVarValue is set the interactable will only be active in case specified variable has the specified value. It is useful in case you want your interactable to act differently depending on conditions (opened and closed door for example), or you only want it to be active in specific cases.

### NK - Change property over time (Number)
Used to change any node number property over time

* `nodeToUpdate {scenenode}` - node to which the change is applied
* `propertyToUpdate {string}` - node property that will be updated. Could be for ex. 'FieldOfView_Degrees', 'Pos X (percent)', 'Pos Y (percent)', 'Width (percent)', 'Height (percent)', 'Pos X (pixels)', 'Pos Y (pixels)', 'Width (pixels)', 'Height (pixels)', 'Alpha', 'Radius', etc
* `startValue {float}` - start value 
* `endValue {float}` - end value
* `duration {int}` - time of transition in ms
* `easeOnStart {boolean}` - check to apply ease function to transition start
* `easeOnEnd {boolean}` - check to apply ease function to transition end
* `actionOnComplete {action}` - action that is executed on transition complete

### NK - Change property over time (Vect3d)
Used to change any node vector 3d property over time

* `nodeToUpdate {scenenode}` - node to which the change is applied
* `propertyToUpdate {string}` - node property that will be updated. Could be for ex. 'Position', 'PositionAbs', 'Scale', 'Rotation', etc.
* `useCurrentValAsStart {boolean}` - use current value as a starting point. Checking this will also ignore `startValue` if set.
* `startValue {vector3d}` - start value 
* `endValue {vector3d}` - end value
* `duration {int}` - time of transition in ms
* `easeOnStart {boolean}` - check to apply ease function to transition start
* `easeOnEnd {boolean}` - check to apply ease function to transition end
* `actionOnComplete {action}` - action that is executed on transition complete
* `forceUpdateStaticCamProperties {boolean}` - this is a workaround to fix static camera behavior. Only apply if you use multiple of those to update static camera target/position in combination with dialog extension. Basically, it sets static camera in current position of player camera.

### NK - Change property over time (Vect3d)
Simulates player pick up item animation. Interactive node will move into player position or position of a target node.

* `duration {int}` - time of transition in ms
* `actionOnComplete {action}` - action that is executed after transition is completed. Typically 'remove intractable'
* `pickUpTargetNode {scenenode}` - allows to specify another destination node except player. The idea is being able to set up a child node under player that serves as a target, so the item doesn't fly right into "player's face".

### NK - Switch to static camera
Activates static camera

### NK - Footsteps
Extension is attached to the player camera. Plays footsteps, tilts camera on strafe and applies some fake camera bob.

* `stepSpeed {float}` - defines the length of one step relative to player speed. If you want more frequent steps set it to less than 1, if you want longer steps set it to higher.
* `playFootSteps {boolean}` - play footsteps or not. It can play multiple sounds in random order. In order for this to work you need to put create 3d sound nodes, attach footstep sounds to them, and put them all in a separate folder.
* `sfxFolder {scenenode}` - folder that contains all footstep sound nodes.
* `fakeBobFovShift {float}` - fake bob simulated by applying fov shift. To disable it set to 0.
* `fakeBobStepTilt {float}` - fake bob simulated by tilting the camera on z axis. To disable set it to 0.
* `strafeTiltAngle {float}` - tilt camera on strafe angle. Additional tilt on z axis that happens in case player strafes right or left. Set to 0 in order to disable.
* `strafeTiltSmooth {float}` - smoothing of strafe tilt coefficient.

### NK - Static smooth vector to UP
Used on static camera in order to get a smoother transition from player camera. The transition could feel a bit janky in case player camera was tilted during interaction. This extension fixes the issue.

### NK - Flow Puzzle
Used to create sequence puzzle there you need to interact with 2d scene nodes in a specific order. Example would be a keycode puzzle, or song of 'A Tale of Birds Without a Voice' from Silent Hill.

* `successCombination {string}` - combination that player has to input to succeed. Could be '1234', or 'cat,dog,mouse'.
* `combinationDelimeter {string}` - combination delimeter. It is nothing by default (''), but you can set it to ',' for example as in 'cat,dog,mouse' example. If you just use characters or number it is not required.
* `errorOnIncorrectCombination {boolean}` - automatically fail in case incorrect combination is input. It is checked in case user input has the same length as expected. Practically, it means user has to clean the input by pressing clear button first.
* `delayAfterPinCheck {int}` - delay after combination gets checked
* `registerButtons {action}` - it a special action there you add "NK - Register Puzzle Flow Key" actions in order to register puzzle buttons
* `actionOnCombinationCheck {action}` - action executed in case combination is getting checked no matter if it is success or fail
* `actionOnSuccess {action}` - action that is executed after combination check success. Typically, show success message or play success sound here.
* `actionOnSuccessAfterCheck {action}` - action that is executed in case of success after combination check delay. Basically, you set your variables and switch scene here.
* `actionOnError {action}` - action executed on combination check fail. Show error message or play error sound here.
* `actionOnKeyPress {action}` - action executed on puzzle key press.
* `clearButton {scenenode}` - node click on which clears the last input
* `submitButton {scenenode}` - node click on which checks combination. If you leave this empty the combination will be checked automatically.
* `inputFieldNode {scenenode}` - node to show current input. Leave empty to not show the input
* `targetAspect {string}` - it's a workaround to fix target aspect of 2d nodes used in the scene. If default puzzle scene background is rendered in 16:9 - specify it as 16:9. Common cases would be 4:3, 16:9, 16:10.
* `widthFixFolder {string}` - name of the folder containing all 2d nodes that should have adaptive aspect ratio.

### NK - Register Puzzle Flow Key
Used inside "registerButtons" property of "NK - Flow Puzzle" behavior to register nodes as sequence puzzle buttons.

* `sceneNode {scenenode}` - a node to register
* `valueToInput {string}` - a value that is input in case user clicks on the button
