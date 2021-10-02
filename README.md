# Blackjack

## Blackjack 주요 함수

+ pygame_menu 모듈을 통한 추가적인 개발
```
menu = pygame_menu.Menu('Welcome', 400, 300, theme=pygame_menu.themes.THEME_BLUE)
name = menu.add.text_input('Name: ', default='Player')
menu.add.selector('Difficulty: ', [('Easy', 1), ('Hard', 2)], onchange=set_difficulty)
menu.add.button('Play', start_the_game)
menu.add.button('Quit', pygame_menu.events.EXIT)
```
게임을 시작할 때 게임을 바로 진행하는 것이 불편할 수 있기 때문에 시작 화면을 따로 구현하여 확장하였다. 화면에 출력할 이름, 게임의 난이도를 플레이어가 직접 설정할 수 있는 맞춤설정 기능을 지원한다.

+ Blackjack에 필요한 기본 함수를 통해 게임을 제어하는 메인 함수
```
def start_the_game():
    global turn, termination
    while True:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if turn:
                if 350 <= pygame.mouse.get_pos()[0] <= 463 and 230 <= pygame.mouse.get_pos()[1] <= 382:
                    if event.type == pygame.MOUSEBUTTONDOWN:
                        audio('hit.ogg', 0.7)
                        player.shuffle()
                        player.add()
                        turn = False
                if 505 <= pygame.mouse.get_pos()[0] <= 684 and 231 <= pygame.mouse.get_pos()[1] <= 386:
                    if event.type == pygame.MOUSEBUTTONDOWN:
                        audio('stay.ogg', 0.7)
                        turn = False

        clock.tick(fps)
        screen.blit(background, (0, 0))
        pygame.time.delay(300)

        termination = check_win()
        show_cards()
        show_turn(turn)
        turn = computer_turn(selection[len(selection) - 1], turn)

        player.x = 50
        computer.x = 850
        pygame.display.update()
        
menu.mainloop(screen)
```
위 함수를 무한 루프로 설정하여 GUI 화면이 유지되면서 게임을 계속할 수 있도록 하였다.

## Blackjack 개발 흐름

  + 1. Blackjack의 규칙이 무엇인지 인터넷에서 검색하여 필요한 정보를 추출함.
  + 2.
  +
