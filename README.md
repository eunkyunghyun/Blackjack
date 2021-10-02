# Blackjack

### Blackjack 개발

+ pygame_menu 모듈을 통한 추가적인 개발
```
menu = pygame_menu.Menu('Welcome', 400, 300, theme=pygame_menu.themes.THEME_BLUE)
name = menu.add.text_input('Name: ', default='Player')
menu.add.selector('Difficulty: ', [('Easy', 1), ('Hard', 2)], onchange=set_difficulty)
menu.add.button('Play', start_the_game)
menu.add.button('Quit', pygame_menu.events.EXIT)
```
게임을 시작할 때 게임을 바로 진행하는 것이 불편할 수 있기 때문에 시작 화면을 따로 구현하여 확장하였다.
