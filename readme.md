# Workshop Guide: Chicken Jump Game @ SP SoC

#### **Workshop Designer:** Mr Siah Peih Wee

#### **School:** School of Computing

#### **Institute:** Singapore Polytechnic

#### **Duration:** 1 Hour

## **About This Workshop:**
In this hands-on workshop, you will learn how to complete a simple jumping game using PixiJS, a popular JavaScript library for creating interactive graphics. Over the course of one hour, you'll fill in missing code in the `script.js` file, gaining practical experience with game development concepts such as sprite animation, collision detection, and game state management. By the end of the workshop, you'll be able to run your game locally and understand how to deploy it online. This workshop is designed for secondary students with a basic understanding of JavaScript and HTML/CSS.

## **Preview:**
To get an idea of what your completed game will look like, visit the following link: [Chicken Jump Game Preview](https://chicky.netlify.app/). Use this as a reference to guide your development and understand the end goal.

### **Step 0: Download and Open the Project**

#### **1. Download the Project**

1. **Go to the GitHub Repository:**
   - Navigate to the GitHub page where the project is hosted.

2. **Download the ZIP File:**
   - Click on the "Code" button (usually green) near the top right of the repository.
   - Select "Download ZIP" from the dropdown menu.
   - Save the ZIP file to your computer.

3. **Extract the ZIP File:**
   - Locate the downloaded ZIP file on your computer.
   - Right-click on the file and select "Extract All..." (or use a similar extraction tool depending on your operating system).
   - Choose a destination folder and extract the files.

#### **2. Open the Project Folder in VS Code**

1. **Launch VS Code:**
   - Open Visual Studio Code on your computer.

2. **Open the Project Folder:**
   - Click on `File` in the top menu and select `Open Folder...`.
   - Navigate to the location where you extracted the ZIP file.
   - Select the project folder (e.g., `chicky-game-start`) and click `Select Folder` (or `Open`).

#### **3. Understand the File Structure**

Once the project is opened in VS Code, you'll see the following file structure in the Explorer panel:

```
chicky-game-start            
├─ images              
│  ├─ chicken.json     
│  ├─ chicken_run.png  
│  └─ tree.png         
├─ index.html          
├─ readme.md           
├─ script.js           
└─ style.css           
```

- **`images/`**: This folder contains all the image assets for the game.
  - `chicken.json`: A JSON file that includes the animation frames for the chicken sprite.
  - `chicken_run.png`: The PNG file that contains the images for the chicken animation.
  - `tree.png`: The PNG file used for the obstacle in the game.

- **`index.html`**: The HTML file that sets up the game’s web page. It includes the game container and references to the CSS and JavaScript files.

- **`readme.md`**: A Markdown file that usually contains information about the project, such as setup instructions, features, and usage details.

- **`script.js`**: The JavaScript file where you’ll write the game logic. This file will handle the game's functionality, including animations, collision detection, and scoring.

- **`style.css`**: The CSS file that contains the styling for the game. It sets up how the game looks, including the layout and visual appearance.

#### **4. Prepare for Coding**

- **Ensure all files are properly loaded in VS Code.**
- **Start by editing `script.js` to implement the game logic as outlined in the workshop guide.**

This setup will allow you to develop and test your game locally before deploying it.

---

### **Step 1: Understand the Existing Code**
1. **Pixi Application Creation:**
   - The existing code initializes a PixiJS application, setting its width, height, and background color. This is the main stage where your game will run.

   ```javascript
   const app = new PIXI.Application({
       width: 800,
       height: 200,
       backgroundColor: 0x1099bb,
       eventMode: 'passive',
       eventFeatures: {
           move: true,
           globalMove: false,
           click: true,
           wheel: true,
       }
   });
   
   document.getElementById("game").appendChild(app.view);
   ```

   - **Explanation:** The `app` object represents your game area. It’s like the canvas where everything in your game will be drawn.

2. **Game State and Variables:**
   - You need to define variables to keep track of the game state. This includes whether the chicken is jumping, how fast it’s moving, gravity, and the score.

   ```javascript
   let isJumping = false;
   let jumpSpeed = 0;
   const gravity = 0.5;
   const jumpHeight = -10;
   const frames = [];
   let chicky;
   let score;
   let scoreValue = 0;
   const obstacleSpeed = 5;
   ```

### **Step 2: Adding Code to `script.js`**

1. **Set the Initial Game State and Variables**
   - **Task:** Copy and paste the following code under the `Set the initial game state and variables` section.

     ```javascript
     let isJumping = false;
     let jumpSpeed = 0;
     const gravity = 0.5;
     const jumpHeight = -10;
     const frames = [];
     let chicky;
     let score;
     let scoreValue = 0;
     const obstacleSpeed = 5;
     ```

2. **Create the Obstacle Sprite**
   - **Task:** Add code to create an obstacle that the chicken will jump over. Place it under the `Create the obstacle sprite` section.

     ```javascript
     const obstacleTexture = PIXI.Texture.from('images/tree.png');
     const obstacle = new PIXI.Sprite(obstacleTexture);
     ```

   - **Explanation:** This code loads an image of a tree as a texture and creates a sprite from it, which will be your obstacle.

3. **Setup the Scoreboard**
   - **Task:** Display the player’s score. Add the following code under the `Setup the scoreboard` section.

     ```javascript
     const setupScoreboard = () => {
         score = new PIXI.Text('Score: 0', { fill: 0xffffff });
         score.style.fontFamily = 'Pixelify Sans';
         score.x = 10;
         score.y = 10;
         app.stage.addChild(score);
     }
     ```

   - **Explanation:** This creates a text object to show the player's score on the screen.

4. **Setup the Jump Event**
   - **Task:** Allow the player to jump when they press a key or click the screen. Add this code under the `Setup the jump event` section.

     ```javascript
     const setupJump = () => {
         const jump = (event) => {
             if (!isJumping) {
                 isJumping = true;
                 jumpSpeed = jumpHeight;
             }
         }
         window.addEventListener('keydown', jump);
         app.stage.hitArea = new PIXI.Rectangle(0, 0, app.screen.width, app.screen.height);
         app.stage.interactive = true;
         app.stage.on('pointerdown', jump);
     }
     ```

   - **Explanation:** This code listens for a key press or click and makes the chicken jump.

5. **Setup the Obstacle Sprite**
   - **Task:** Position the obstacle on the screen. Add the following code under the `Setup the obstacle sprite` section.

     ```javascript
     const setupObstacle = () => {
         obstacle.width = 30;
         obstacle.height = 60;
         obstacle.x = app.screen.width;
         obstacle.y = app.screen.height - obstacle.height;
         app.stage.addChild(obstacle);
     }
     ```

   - **Explanation:** This code sizes the obstacle and places it at the right edge of the screen.

6. **Move the Obstacle**
   - **Task:** Make the obstacle move towards the chicken. Add this code under the `Move the obstacle` section.

     ```javascript
     const moveObstacle = (deltaTime) => {
         obstacle.x -= obstacleSpeed * deltaTime;
         if (obstacle.x + obstacle.width < 0) {
             obstacle.x = app.screen.width;
             scoreValue++;
             score.text = `Score: ${scoreValue}`;
         }
     }
     ```

   - **Explanation:** The obstacle moves left across the screen, and if it goes off-screen, it resets to the right side, increasing the score.

7. **Setup the Chicken Sprite**
   - **Task:** Create and animate the chicken character. Add this code under the `Setup the chicken sprite` section.

     ```javascript
     const setupChicken = (frames) => {
         chicky = new PIXI.AnimatedSprite(frames);
         chicky.animationSpeed = 0.1;
         chicky.play();
         chicky.width = 32;
         chicky.height = 32;
         chicky.x = 50;
         chicky.y = app.screen.height - chicky.height;
         app.stage.addChild(chicky);
     }
     ```

   - **Explanation:** The chicken is an animated sprite that will run continuously. It’s placed near the bottom left of the screen.

8. **Move the Chicken**
   - **Task:** Make the chicken jump and fall back down due to gravity. Add this code under the `Move the chicken` section.

     ```javascript
     const moveChicken = (deltaTime) => {
         if (isJumping) {
             chicky.y += jumpSpeed * deltaTime;
             jumpSpeed += gravity * deltaTime;
             if (chicky.y >= app.screen.height - chicky.height) {
                 chicky.y = app.screen.height - chicky.height;
                 isJumping = false;
             }
         }
     }
     ```

   - **Explanation:** This handles the jumping physics, allowing the chicken to go up and come back down.

9. **Check for Collision**
   - **Task:** End the game if the chicken hits the obstacle. Add this code under the `Check for collision` section.

     ```javascript
     const checkCollision = () => {
         if (
             chicky.x < obstacle.x + obstacle.width &&
             chicky.x + chicky.width > obstacle.x &&
             chicky.y < obstacle.y + obstacle.height &&
             chicky.y + chicky.height > obstacle.y
         ) {
             alert("Game Over!");
             obstacle.x = app.screen.width;

             scoreValue = 0;
             score.text = `Score: ${scoreValue}`;
         }
     }
     ```

   - **Explanation:** This checks if the chicken has collided with the obstacle and resets the game if it has.

10. **Load the Chicken Animation Sprite Sheet**
    - **Task:** Load the chicken sprite sheet and start the game loop. Add this code under the `Load the Chicken animation sprite sheet` section.

      ```javascript
      const texturePromise = PIXI.Assets.load('images/chicken.json');
      texturePromise.then((resolvedTexture) => {
          for (let i = 0; i < resolvedTexture._frameKeys.length; i++) {
               frames.push(resolvedTexture.textures[resolvedTexture._frameKeys[i]]);
          }

          setupChicken(frames);
          setupJump();
          setupObstacle();
          setupScoreboard();

          app.ticker.add((deltaTime) => {
              moveChicken(deltaTime);
              moveObstacle(deltaTime);
              checkCollision();
          });
      });
      ```

    - **Explanation:** This loads the chicken’s animation frames, sets up the game objects, and starts the game loop.

---

### **Step 3: Ensure the JavaScript Code is Completed**
1. **Review `script.js` in your code editor** and ensure the code is the same as below:
    ```javascript
    // ////////////////////////////////////////////////////////////
    // Create a Pixi Application
    // ////////////////////////////////////////////////////////////
    const app = new PIXI.Application({
        width: 800,
        height: 200,
        backgroundColor: 0x1099bb,
        eventMode: 'passive',
        eventFeatures: {
            move: true,
            globalMove: false,
            click: true,
            wheel: true,
        }
    });

    document.getElementById("game").appendChild(app.view);

    // ////////////////////////////////////////////////////////////
    // Set the initial game state and variables
    // ////////////////////////////////////////////////////////////
    let isJumping = false;
    let jumpSpeed = 0;
    const gravity = 0.5;
    const jumpHeight = -10;
    const frames = [];
    let chicky;
    let score;
    let scoreValue = 0;
    const obstacleSpeed = 5;

    // ////////////////////////////////////////////////////////////
    // Create the obstacle sprite
    // ////////////////////////////////////////////////////////////
    const obstacleTexture = PIXI.Texture.from('images/tree.png');
    const obstacle = new PIXI.Sprite(obstacleTexture);

    // ////////////////////////////////////////////////////////////
    // Setup the scoreboard
    // ////////////////////////////////////////////////////////////
    const setupScoreboard = () => {
        score = new PIXI.Text('Score: 0', { fill: 0xffffff });
        score.style.fontFamily = 'Pixelify Sans';
        score.x = 10;
        score.y = 10;
        app.stage.addChild(score);
    }

    // ////////////////////////////////////////////////////////////
    // Setup the jump event
    // ////////////////////////////////////////////////////////////
    const setupJump = () => {
        const jump = (event) => {
            if (!isJumping) {
                isJumping = true;
                jumpSpeed = jumpHeight;
            }
        }
        window.addEventListener('keydown', jump);
        app.stage.hitArea = new PIXI.Rectangle(0, 0, app.screen.width, app.screen.height);
        app.stage.interactive = true;
        app.stage.on('pointerdown', jump);
    }

    // ////////////////////////////////////////////////////////////
    // Setup the obstacle sprite
    // ////////////////////////////////////////////////////////////
    const setupObstacle = () => {
        obstacle.width = 30;
        obstacle.height = 60;
        obstacle.x = app.screen.width;
        obstacle.y = app.screen.height - obstacle.height;
        app.stage.addChild(obstacle);
    }

    // ////////////////////////////////////////////////////////////
    // Move the obstacle
    // ////////////////////////////////////////////////////////////
    const moveObstacle = (deltaTime) => {
        obstacle.x -= obstacleSpeed * deltaTime;
        if (obstacle.x + obstacle.width < 0) {
            obstacle.x = app.screen.width;
            scoreValue++;
            score.text = `Score: ${scoreValue}`;
        }
    }

    // ////////////////////////////////////////////////////////////
    // Setup the chicken sprite
    // ////////////////////////////////////////////////////////////
    const setupChicken = (frames) => {
        chicky = new PIXI.AnimatedSprite(frames);
        chicky.animationSpeed = 0.1;
        chicky.play();
        chicky.width = 32;
        chicky.height = 32;
        chicky.x = 50;
        chicky.y = app.screen.height - chicky.height;
        app.stage.addChild(chicky);
    }

    // ////////////////////////////////////////////////////////////
    // Move the chicken
    // ////////////////////////////////////////////////////////////
    const moveChicken = (deltaTime) => {
        if (isJumping) {
            chicky.y += jumpSpeed * deltaTime;
            jumpSpeed += gravity * deltaTime;
            if (chicky.y >= app.screen.height - chicky.height) {
                chicky.y = app.screen.height - chicky.height;
                isJumping = false;
            }
        }
    }

    // ////////////////////////////////////////////////////////////
    // Check for collision
    // ////////////////////////////////////////////////////////////
    const checkCollision = () => {
        if (
            chicky.x < obstacle.x + obstacle.width &&
            chicky.x + chicky.width > obstacle.x &&
            chicky.y < obstacle.y + obstacle.height &&
            chicky.y + chicky.height > obstacle.y
        ) {
            alert("Game Over!");
            obstacle.x = app.screen.width;

            scoreValue = 0;
            score.text = `Score: ${scoreValue}`;
        }
    }

    // ////////////////////////////////////////////////////////////
    // Load the Chicken animation sprite sheet
    // ////////////////////////////////////////////////////////////
    const texturePromise = PIXI.Assets.load('images/chicken.json');
    texturePromise.then((resolvedTexture) => {
        for (let i = 0; i < resolvedTexture._frameKeys.length; i++) {
             frames.push(resolvedTexture.textures[resolvedTexture._frameKeys[i]]);
        }

        setupChicken(frames);
        setupJump();
        setupObstacle();
        setupScoreboard();

        app.ticker.add((deltaTime) => {
            moveChicken(deltaTime);
            moveObstacle(deltaTime);
            checkCollision();
        });
    });
    ```

### **Step 4: Testing Your Game Locally with VS Code**

1. **Install the Live Server Extension:**
   - Open VS Code.
   - Go to the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of the window or by pressing `Ctrl+Shift+X`.
   - In the search bar, type "Live Server" and press Enter.
   - Find the "Live Server" extension by Ritwick Dey and click the "Install" button.

2. **Start Live Server:**
   - Right-click on your `index.html` file in the Explorer panel.
   - Select "Open with Live Server" from the context menu.

3. **Test Your Game:**
   - Your game will open in a web browser.
   - Ensure the game loads correctly, and you can jump over the obstacle using either a mouse click or the spacebar.

### **Step 5: Deploying to Netlify**
1. **Go to [Netlify](https://www.netlify.com/)** and sign up if you don’t have an account.
2. **Drag and Drop Your Project Folder:**
   - Once logged in, drag and drop your `pixi-game` folder into the Netlify dashboard.
3. **Wait for Deployment:**
   - Netlify will automatically deploy your site, and you’ll get a link to access your game online.
4. **Test Your Game Online:**
   - Visit the link provided by Netlify to see your game live!

### **Step 6: Share Your Game**
- **Share the Netlify link** with your friends, family, or teachers to show off your creation!

---

### **Additional Resources:**
- [PixiJS Documentation](https://pixijs.com/)
- [Netlify Documentation](https://docs.netlify.com/)

---

**End of Workshop Guide**