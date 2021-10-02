# Blackjack

## Blackjack 참조 이미지

### Blackjack 시작 화면
![블랙잭1](https://user-images.githubusercontent.com/57312000/135707257-1c4f9282-5261-409c-a23e-730a5972abec.PNG)

### Blackjack 진행 화면
![블랙잭2](https://user-images.githubusercontent.com/57312000/135707259-c062cd59-6959-48db-ad79-c4d988d735d5.PNG)

### Blackjack 승패 결정 화면
![블랙잭3](https://user-images.githubusercontent.com/57312000/135707261-2f7244ba-1d51-4568-9b54-de2fad823f98.PNG)

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
  + 4. 게임을 완성하여 GitHub에 업로드함.

## Blackjack 개발 중 발생한 오류

  + 기대치 계산 오류
    카드는 a부터 k까지 각각 3개가 주어지며 플레이어와 컴퓨터가 카드를 가져올 때마다 주어진 카드의 전체 양은 감소한다. 이것을 고려했을 때 기댓값의 공식은 다음과 같다.
    
    ![공식](https://user-images.githubusercontent.com/57312000/135707659-58eaec5a-7c47-4c48-9138-726c72f26374.PNG)
    
    이미지에 등장한 values[i], cards[i]는 아래 리스트에서 유래하였다.
    ```
    values = {'a': 1, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, 't': 10, 'j': 10, 'q': 10, 'k': 10}
    cards = {'a': 3, '1': 3, '2': 3, '3': 3, '4': 3, '5': 3, '6': 3, '7': 3, '8': 3, '9': 3, 't': 3, 'j': 3, 'q': 3, 'k': 3}
    ```
    즉 '해당 카드의 값 × 해당 카드의 수 ÷ 전체 카드의 수'의 값을 모두 합산한 것이 기댓값이 되는 것이다. 그런데 여기서 기댓값의 개념을 잘못 이해하여 '해당 카드의 수 ÷ 전체 카드의 수'를 모두 더한 것이 기댓값이 되는 줄 알았다. 결국 1과 가까운 숫자만 기댓값이라는 결과가 도출되었다. 각 카드의 점수를 무시하고 기댓값을 계산한 셈이다. 계산 오류를 파악하고 몇 번의 직접적인       계산 끝에 마침내 계산 오류를 해결할 수 있었다.

## Blackjack 업데이트 안내
  
  1. Blackjack에 pygame_menu 모듈을 통해 시작 화면을 구현할 예정입니다. **(2021.09.11 업데이트 완료)**
  2. Difficult 난이도에서 컴퓨터 차례 때 컴퓨터가 기댓값을 계산하여 Hit을 선택할 지, Stay를 선택할 지 판단할 수 있는 기능을 개발할 예정입니다. **(2021.09.25 업데이트 완료)**
  3. Hit을 선택했을 때 카드가 패널로 이동하는 모션을 보여주는 기능을 추가할 예정입니다. **(업데이트 미완료)**
