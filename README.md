using System;
using System.Collections.Generic;
using System.Linq;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Audio;
using Microsoft.Xna.Framework.Content;
using Microsoft.Xna.Framework.GamerServices;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Input;
using Microsoft.Xna.Framework.Media;

namespace BallBounce
{
    /// <summary>
    /// This is the main type for your game
    /// </summary>
    public class Game1 : Microsoft.Xna.Framework.Game
    {
        GraphicsDeviceManager graphics;
        SpriteBatch spriteBatch;
        Texture2D ballTexture;
        Rectangle currentBall;
        float timeRemining = 0.3f;
        const float TimePerBall = 0.3f;
        int bounce = 0;
        Color[] colors = new Color[3] { Color.Red, Color.Green, Color.Blue };

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            Content.RootDirectory = "Content";
        }

        /// <summary>
        /// Allows the game to perform any initialization it needs to before starting to run.
        /// This is where it can query for any required services and load any non-graphic
        /// related content.  Calling base.Initialize will enumerate through any components
        /// and initialize them as well.
        /// </summary>
        protected override void Initialize()
        {
            // TODO: Add your initialization logic here

            this.IsMouseVisible = true;

            base.Initialize();
        }

        /// <summary>
        /// LoadContent will be called once per game and is the place to load
        /// all of your content.
        /// </summary>
        protected override void LoadContent()
        {
            // Create a new SpriteBatch, which can be used to draw textures.
            spriteBatch = new SpriteBatch(GraphicsDevice);

            ballTexture = Content.Load<Texture2D>(@"circle");
            // TODO: use this.Content to load your game content here
        }

        /// <summary>
        /// UnloadContent will be called once per game and is the place to unload
        /// all content.
        /// </summary>
        protected override void UnloadContent()
        {
            // TODO: Unload any non ContentManager content here
        }

        /// <summary>
        /// Allows the game to run logic such as updating the world,
        /// checking for collisions, gathering input, and playing audio.
        /// </summary>
        /// <param name="gameTime">Provides a snapshot of timing values.</param>
        protected override void Update(GameTime gameTime)
        {
            // Allows the game to exit
            if (GamePad.GetState(PlayerIndex.One).Buttons.Back == ButtonState.Pressed)
                this.Exit();

            // TODO: Add your update logic here
            int xChange = 1;
            int yChange = 1;

            MouseState mouse = Mouse.GetState();
            if (mouse.LeftButton == ButtonState.Pressed)
                currentBall = new Rectangle(mouse.X, mouse.Y, 25, 25);

            if (currentBall.X == 25)
            {
                xChange = -1;
                bounce++;
            }
            else if (currentBall.X == 0)
            {
                xChange = 1;
                bounce++;
            }
            else if (currentBall.Y == 25)
            {
                yChange = -1;
                bounce++;
            }
            else if (currentBall.Y == 0)
            {
                yChange = 1;
                bounce++;
            }


            if(timeRemining == 0.0f)
            {
                currentBall.X = currentBall.X + xChange;
                currentBall.Y = currentBall.Y + yChange;
                currentBall = new Rectangle(currentBall.X, currentBall.Y, 25, 25);

                timeRemining = TimePerBall;
            }

            timeRemining = MathHelper.Max(0, timeRemining - (float)gameTime.ElapsedGameTime.TotalSeconds);

            this.Window.Title = "Bounce: " + bounce.ToString();

            base.Update(gameTime);
        }

        /// <summary>
        /// This is called when the game should draw itself.
        /// </summary>
        /// <param name="gameTime">Provides a snapshot of timing values.</param>
        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.Gray);

            // TODO: Add your drawing code here

            spriteBatch.Begin(); 
            spriteBatch.Draw(
                ballTexture,
                currentBall,
                colors[bounce % 3]);

            base.Draw(gameTime);
        }
    }
}

