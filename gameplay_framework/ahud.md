# AHUD

The class AHUD is only available on each client and can be accessed through the PlayerController. It will be automatically spawned by the PlayerController.

Before UMG (Unreal Motion Graphics) had been released, the AHUD class has been used to draw text, textures and more in the viewport of the client.

Nowadays UserWidgets replaced the HUD class 99% of the time.

You can still use the AHUD Class to debug or maybe have an isolated area to manage to create, show, hide and destroy UserWidgets.

Since the HUD isn't directly linked to multiplayer, examples would only show singleplayer logic, so we will skip them for this class.
