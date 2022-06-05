# studbud
# Feedback and Iteration
> "A few of your design ideas go against persona frsutrations. i.e. doesn't like loud notification noises. How does that tie in with a loud motivational video at the end of the timer? The red background is also quite scary." _Tutor_

Design has a significant focus on iteration and improvement, a key attribute of what makes it timeless and relevant. Feedback on assessment 2 included novel ideas may be too far out for people to get on board with, jarring use of bright red colours may be overwhelming, and loud sounds go against the persona frustrations of enjoying peace and simplicity. Upon receiving feedback on my web designs, I immediately sought to engage with the iteration process.

![comparison between mockup and prototype](docs/comparison.PNG)

The mockup and web prototype differ as I thought it best that keeping a simple web page without having to click into external pages to access the task list, would make the design appear clean and easily navigatable. Using __particles.js__ I altered the javascript to make the particles slower, float to top, is unlinked and is smaller. This mimics bubbles which carries on aspects of my initial design idea of a fish aquarium, but is toned down as to not be confusing. Thus, this creates a pleasing background.
### JavaScript
```JS
     "line_linked": {
        "enable": false,
        "distance": 150,
        "color": "#ffffff",
        "opacity": 0.4,
        "width": 1
      },
      "move": {
        "enable": true,
        "speed": 1,
        "direction": "top",
        "random": false,
        "straight": false,
        "out_mode": "out",
        "attract": {
          "enable": false,
          "rotateX": 600,
          "rotateY": 1200
        }
      }
```
The mainpage maintained the key component of the pomodoro timer being situated in the middle of the webpage when being translated from mockup to prototype. However, to maintain the illusion of a larger screen, I opted for __transparent and semi transparent backgrounds__ for the pomodoro timer, dictionary, task todo list, and media player. This provides a background for text to be read against as well as help distinguish components of the web application from eacher. Therefore, __improving legibility, contrast and visibility.__ 

A __sticky progress bar__ was created by fixing its position to the top of the web page. This allows it to be accessible and viewable throughout the scrolling process when the web app is viewed in from various device screens that require to scroll down. This bar visually shows the elapsed time from when the pomodoro timer started as well as a visual indicator of the amount of time remaining. 
### CSS
```CSS
/* Progress bar at top of page to show visial indication of elapsed time */
  progress {
    border-radius: 2px;
    width: 100%;
    height: 12px;
    position: fixed; 
    top: 0;
    left: 0;
    right: 0;
  }
  progress::-webkit-progress-bar {
   background-color: rgba(0, 0, 0, 0.1);;
  }
  progress::-webkit-progress-value {
    background-color: #fff;
  }
```

I have also deisgned a simple StudBud logo and favicon that are visible on the webpage and browser.
![Studbud logo and favicon](docs/logo.PNG)

# Reconfiguration for Responsivity
A responsive website has a fluid and flexible layout that adjusts to various screen sizes. This is crucial in offering an __optimised browsing experience.__ To ensure elements would be able to adjust to specific screens, there was a lot of focus on relative positioning. I also scaled containers, images, backgrounds, margins and text to various mobile resolutions ranging from 1140px to 320px as to ensure responsiveness. 

On horizontal surfaces, people read in a left to right fashion, therefore, landscape view was dominant in desktop and laptop versions of the design. The landscape format enlarges the screen and field of view, making it more dynamic. However, as the website adjusts to smaller devices such as tablets and phones, elements are arranged in columns rather than rows as content on these devices are habitually read top to bottom. Thus, several text elements and containers are centered rather than left aligned, allowing it to be compact on a mobile screen.

When the screen size is less than 768px, the task list moves position form being on the left-hand side to the centre. This is to accomodate the smaller screen size and __scrolling__ as the pomodoro timer is the main attraction. It is a simple, compact design. 
### CSS
```CSS
@media screen and (max-width:768px) {
  .clock {
    font-size: 10rem;
  }
  .timer {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 20px;
    text-align: center;
  }
  .main-button {
    font-size: 20px;
    width: 200px;
  }
  .shell{
    width: auto;
    margin: 700px auto;
    left: 5px;
    right: 5px;
    position: absolute;
    background: rgba(255, 255, 255, 0.781);
    border-radius: 7px;
   }
}
  .music-container {
    background-color: rgba(255, 255, 255, 0.781);
    border-radius: 15px;
    box-shadow: 0 20px 20px 0 rgba(37, 46, 75, 0.384);
    display: flex;
    padding: 20px 30px;
    position: relative;
    z-index: 10;
    max-width: fit-content;
    bottom: 30%;
    width: auto;
    transform: translate(15%, 0%);
  }
```

# Components
## 1. Task List
Users are classically conditioned to read left to right. Therefore, the dictionary and task list have been placed on the left to accomodate this reading pattern. In the process of study, the user will outline their tasks before moving onto the pomodoro session.

The task list uses local storage to save user inputs and is allocated matching id so that it can be categorised. This helps to create __convey quadrants__.The user can name a new task, categorise, set a due date, and allocate time. The user can sort through tasks based on assigned category as well as priority in related to time. Once completed, they can check the box as well as deleted tasks that are no longer needed. 
### JS
```JS
 function save() {
    let stringified = JSON.stringify(todoList);
    localStorage.setItem("todoList", stringified);
  }

  function load() {
    let retrieved = localStorage.getItem("todoList");
    todoList = JSON.parse(retrieved);
    //console.log(typeof todoList)
    if (todoList == null)
      todoList = [];
  }

  function renderRows() {
    todoList.forEach(todoObj => {
      rendowRow(todoObj);
    })
  }

```
## 2. Dictionary
The dictionary was made using a dictionary __API__. This allow access to word data libraries and integrate word definitions, synonyms and sentence examples. With an included dictionary, the user can minimise the need to open new tabs to search for words. Hence, it provides __ease of access__.
### JavaScript
```JS
function search(word){
    fetchApi(word);
    searchInput.value = word;
}
function fetchApi(word){
    wrapper.classList.remove("active");
    infoText.style.color = "#000";
    infoText.innerHTML = `Searching for the meaning of <span>"${word}"</span>`;
    let url = `https://api.dictionaryapi.dev/api/v2/entries/en/${word}`;
    fetch(url).then(response => response.json()).then(result => data(result, word)).catch(() =>{
        infoText.innerHTML = `What I found for <span>"${word}"</span>`;
    });
}
searchInput.addEventListener("keyup", e =>{
    let word = e.target.value.replace(/\s+/g, ' ');
    if(e.key == "Enter" && word){
        fetchApi(word);
    }
});
```
## 3. Pomodoro
A __visual que__ is highlighted through the eased in color changes in the background to indicate a different pomodoro mode has been selected. When the timer expires for any mode, the timer resets to the pomodoro session as default. To further highlight the varying modes, a __text notification__ is printed into the tab of the webrower, displaying time remaining in session as well as a corresponding message. This small detail is a factor that can reduce website bounce rates as it will attract the user.
### JavaScript
```JS
function startTimer() {
  let { total } = timer.remainingTime;
  const endTime = Date.parse(new Date()) + total * 1000;

  if (timer.mode === 'pomodoro') timer.sessions++;

  mainButton.dataset.action = 'stop';
  mainButton.textContent = 'stop';
  mainButton.classList.add('active');

  interval = setInterval(function() {
    timer.remainingTime = getRemainingTime(endTime);
    updateClock();

    total = timer.remainingTime.total;
    if (total <= 0) {
      clearInterval(interval);

      switch (timer.mode) {
        case 'pomodoro':
          if (timer.sessions % timer.longBreakInterval === 0) {
            switchMode('longBreak');
          } else {
            switchMode('shortBreak');
          }
          break;
        default:
          switchMode('pomodoro');
      }

      if (Notification.permission === 'granted') {
        const text =
          timer.mode === 'pomodoro' ? 'Time to study hard!' : 'Take a break!';
        new Notification(text);
      }

      document.querySelector(`[data-sound="${timer.mode}"]`).play();

      startTimer();
    }
  }, 1000);
}
```
## 4. Music Player
The music player is situated in the bottom right corner when the web page is viewed in landscape mode, or it it cemtred when viewed on a mobile device. The vinyl images retrieved from __Unsplash__ rotate when the music player is played, and stops rotating when the player is stopped. The transitions were created using CSS and the animations were completed in JavaScript. The user can listen to three songs with the currently playing song being displayed in a translated tab that is revealed when the music is started.
### JavaScript
```JS
// Song titles
const songs = [ 'Stratlove - Nils Baumgartel', 'Silent Night Beats To Study To - Aves', 'Gotta Keep Goin - Milano' ];

// Keep track of song
let songIndex = 2;

// Initially load song details into DOM
loadSong(songs[songIndex]);

// Update song details
function loadSong(song) {
	title.innerText = song;
	audio.src = `music/${song}.mp3`;
	cover.src = `img/${song}.jpg`;
}

//Play Song
function playSong() {
	musicContainer.classList.add('play');
	playBtn.querySelector('i.fas').classList.remove('fa-play');
	playBtn.querySelector('i.fas').classList.add('fa-pause');

	audio.play();
}
// Pause song
function pauseSong() {
	musicContainer.classList.remove('play');
	playBtn.querySelector('i.fas').classList.add('fa-play');
	playBtn.querySelector('i.fas').classList.remove('fa-pause');

	audio.pause();
}

//Previous Songs
function prevSong() {
	songIndex--;

	if (songIndex < 0) {
		songIndex = songs.length - 1;
	}
	loadSong(songs[songIndex]);

	playSong();
}

//Next Songs
function nextSong() {
	songIndex++;

	if (songIndex > songs.length - 1) {
		songIndex = 0;
	}
	loadSong(songs[songIndex]);

	playSong();
}
```


# References
Artlist. (2020). “Royalty-Free Music & SFX for Video Creators | Artlist.” Retrieved June 4, 2022 (https://artlist.io/?utm_source=google&utm_medium=cpc&utm_campaign=17102212441&utm_content=131361293490&utm_term=free%20music&keyword=free%20music&ad=595608168369&matchtype=e&device=c&gclid=Cj0KCQjwheyUBhD-ARIsAHJNM-OyQnqfDPBOlSPV6CgLWD-WPcZhxmuCtZMzdq84AxtF_heMVPgjvVIaAuGaEALw_wcB).

CodingNepal. (2021). Build A Dictionary App in HTML CSS & JavaScript. Retrieved from: https://www.youtube.com/watch?v=uqgCF3JIHkA&t=109s

CSS text-align property. (2022). Retrieved June 3, 2022, from https://www.w3schools.com/cssref/pr_text_text-align.ASP

Dani Krossing. (2017). Add A Favicon to A Website in HTML | Learn HTML and CSS | HTML Tutorial | HTML for Beginners. Retrieved from: https://www.youtube.com/watch?v=kEf1xSwX5D8

Font Awesome. (2022). Retrieved June 3, 2021, from https://fontawesome.com

Garreau, Vincent. (n.d.). “Particles.Js - A Lightweight JavaScript Library for Creating Particles.” Retrieved from:https://vincentgarreau.com/particles.js/

Google Fonts. (2022). Retrieved June 3, 2022 (https://fonts.google.com/).

How to Center in CSS - EASY ( Center Div and Text Vertically and Horizontally )—YouTube. (n.d.). Retrieved November 21, 2021, from https://www.youtube.com/watch?v=QdITQ4upjME

Kevin Powell. (2016). HTML & CSS for Beginners Part 13: Background Images. Retrieved from https://www.youtube.com/watch?v=33IinMVJf-M

Particles.Js. Retrieved June 5, 2022 (https://vincentgarreau.com/particles.js/).
Online Tutorials. (2017). Bubbles Animation Using Particles.Js - How to Use Particles.Js - Background Particles Animation. Retrieved from:https://www.youtube.com/watch?v=ra5S13bK7Z8&t=1s

Unsplash. (2022). Vinyl. Retrieved from https://www.unsplash.com

W3Schools online HTML editor. (2022). Retrieved June 3, 2022, from https://www.w3schools.com/cssref/tryit.asp?
filename=trycss_text-align_all