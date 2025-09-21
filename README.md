int x, y;
int w = 100;
int h = 0;
int count = 0;
int stage = 6;
int anime = 0;

int touchX, touchY;
boolean isTouching = false;
boolean gameOver = false;

void setup() {
  fullScreen(); 
  x = int(random(100, width - 100));
  y = int(random(100, height - 100));
}

void draw() {
  background(0);

  int currentsize = w + (frameCount % 40 < 20 ? 5 : -5);

  int px = isTouching ? touchX : width / 2;
  int py = isTouching ? touchY : height / 2;

  int dx = px - x;
  int dy = py - y;
  int d = int(sqrt(dx * dx + dy * dy));

  if (d < currentsize / 2 && stage > 0) {
    count++;
    anime = 0;
    x = int(random(100, width - 100));
    y = int(random(100, height - 100));
    if (count % 5 == 0 && stage < 10) stage++;
  } 
  else if (d < currentsize + 40 && stage > 0) {
    anime++;
    if (anime > 30) {
      anime = 0;
      h++;
      if (count > 0) count--;
      if (count % 1 == 4 && stage > 20) stage--;
      if (count <= 0) gameOver = true;
    }
  }

  if (d < 150 && stage > 0) {
    if (px < x) x += stage;
    else x -= stage;
    if (py < y) y += stage;
    else y -= stage;
    x = constrain(x, 50, width - 50);
    y = constrain(y, 50, height - 50);
  }

  noStroke();
  fill(255);
  ellipse(x, y, currentsize, currentsize);

  if (isTouching) {
    stroke(255);
    line(touchX - 10, touchY, touchX + 10, touchY);
    line(touchX, touchY - 10, touchX, touchY + 10);
  }

  fill(255);
  textSize(30);

  if (!gameOver) {
    text("score: " + count, 20, 40);
    text("miss: " + h, 20, 70);
    text("speed: " + stage, 20, 100);
  } else {
    textSize(50);
    fill(255, 0, 0);
    text("GAME OVER", width / 2 - 150, height / 2);
    fill(255);
    textSize(35);
    text("last score: " + count, width / 2 - 120, height / 2 + 60);
    fill(0, 255, 0);
    rect(width / 2 - 100, height / 2 + 100, 200, 60);
    fill(0);
    textSize(30);
    text("RESTART", width / 2 - 60, height / 2 + 140);
  }
}

void touchStarted() {
  isTouching = true;
  touchX = mouseX;  
  touchY = mouseY;
  if (gameOver) {
    if (mouseX > width / 2 - 100 && mouseX < width / 2 + 100 &&
        mouseY > height / 2 + 100 && mouseY < height / 2 + 160) {
      resetGame();
    }
  }
}

void touchMoved() {
  touchX = mouseX;
  touchY = mouseY;
}

void touchEnded() {
  isTouching = false;
}

void resetGame() {
  x = int(random(100, width - 100));
  y = int(random(100, height - 100));
  w = 100;
  h = 0;
  count = 0;
  stage = 6;
  anime = 0;
  gameOver = false;
}
