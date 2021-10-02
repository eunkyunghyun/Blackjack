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

+ 기댓값 계산을 착안하여 개발한 컴퓨터 작동 함수
```
def computer_turn(difficulty, order):
    if not termination:
        if not order:
            if difficulty == 1:
                computer.shuffle()
                computer.add()
                order = True
            else:
                expectancy = 0
                for i in range(len(ranks)):
                    expectancy = expectancy + values[ranks[i]] * cards[ranks[i]] / sum(cards.values())
                if computer.score + expectancy <= 21:
                    computer.shuffle()
                    computer.add()
                order = True
        for i in range(3):
            img = pygame.image.load("functions/hit.png").convert_alpha()
            img = pygame.transform.scale(img, (100, 150))
            screen.blit(img, (350 + 6 * i, 230))
        img = pygame.image.load("functions/stay.png").convert_alpha()
        img = pygame.transform.scale(img, (250, 200))
        screen.blit(img, (SCREEN_WIDTH // 2 - 30, SCREEN_HEIGHT // 2 - 80))
    return order
```

+ Pygame 모듈을 사용하여 GUI 방식으로 게임을 제어하는 메인 함수
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
  + 2. 기본 규칙을 Python 함수로 구현하여 CLI 방식으로 Blackjack을 개발함.
  + 3. CLI 방식에서 확장하여 GUI 방식으로 개발함.
  + 4. GUI 방식의 Blackjack에 pygame_menu, 기대치 계산을 고안하여 추가함.
  + 5. 게임을 완성하여 GitHub에 업로드함.

## Blackjack 개발 중 발생한 오류

  * 1. 기대치 계산 오류
