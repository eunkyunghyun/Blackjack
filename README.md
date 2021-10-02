# Blackjack

### Blackjack 개발

+ Blackjack 함수
```
def audio(name, wait):
    pygame.mixer.music.load(name)
    pygame.mixer.music.play()
    time.sleep(wait)
    pygame.mixer.music.stop()
```
Hit, Stay 버튼을 클릭했을 때 사용하는 효과음을 출력하는 함수이다.

```
def finish(message):
    text = font.render(message, True, black)
    textRect = text.get_rect()
    textRect.centerx = screen.get_rect().centerx
    textRect.centery = screen.get_rect().centery - 40
    screen.blit(text, textRect)
```
게임이 끝났을 때 화면의 중앙에 메시지를 출력하는 함수이다.
