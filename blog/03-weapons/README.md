_This is the weapon's update for v0.4 of Space War RL! I added phasers and photon torpedoes_

## Making a Common Base Class

The `HumanShip` class contains the logic for applying screen wrap-around, velocity, and rotation.
Since I want the other objects to have this same logic, I can extract this into a base class.

```python
class SpaceEntity(pygame.sprite.Sprite):
    """A base class for objects that move in space,
    like ships and projectiles. State is manged internally.

    Attributes:
        surf: The reference to the original Surface, used for rotation
        image: The reference to the latest surface based on the other
               state vars
        pos: The x,y of the position on the screen
        rect: The rect object
        vel: The velocity of the space entity
        ang: The angle in degrees representing the direction the ship is facing
    """

    surf: pygame.surface.Surface
    image: pygame.surface.Surface
    pos: tuple[int, int]
    rect: pygame.rect.Rect
    vel: tuple[float, float]
    ang: float

    def __init__(self, surf, start_pos, start_ang=0, start_vel=(0, 0)) -> None:
        super().__init__()
        self.surf = surf
        self.image = surf
        self.pos = start_pos
        self.rect = surf.get_rect(center=self.pos)
        self.vel = start_vel
        self.ang = start_ang

    def update(self, *_args, **_kwargs):
        """Entrypoint for updating the player state each frame"""
        x_pos, y_pos = self.pos
        x_vel, y_vel = self.vel

        # Conditions for wrapping around the screen
        if x_pos >= SCREEN_WIDTH:
            x_pos = 0
        elif x_pos <= 0:
            x_pos = SCREEN_WIDTH
        if y_pos >= SCREEN_HEIGHT:
            y_pos = 0
        elif y_pos <= 0:
            y_pos = SCREEN_HEIGHT

        # Apply velocity to position
        x_pos += x_vel
        y_pos += y_vel
        self.rect.x = x_pos
        self.rect.y = y_pos
        self.pos = (x_pos, y_pos)

        # Update rotation to surface
        self.ang %= 360
        self.image = pygame.transform.rotate(self.surf, -self.ang)
        self.rect = self.image.get_rect(center=self.pos)

```

## Adding Photon Torpedoes

## Adding Phasers

## Next Steps

I was originally planning to start training agents after implementing energy and shields management, but I have enough features implemented to get started.