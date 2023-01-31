def isStrike(pin1):
    if pin1 == 10:
        return True
    else:
        return False


def isSpare(pin1, pin2):
    if (pin1 + pin2) == 10:
        return True
    else:
        return False


def calculateRoundTotalScore(strikeHolder, spareHolder, pin1, pin2):
    if strikeHolder:
        return 10+pin1+pin2

    elif spareHolder:
        return 10+pin1

    else:
        return pin1 + pin2


def calculateTotalScore(scoreArr, totalScoreArr):
    if len(totalScoreArr) > 0:
        totalScoreArr.append(scoreArr[-1] + totalScoreArr[-1])

    else:
        totalScoreArr.append(scoreArr[-1])


def printScores(scoreArr, totalScoreArr):
    lineCount = 9

    ballScores = 'Ball Scores: '
    totalScores = ['Total Scores: ']
    totalScoresStr = ''
    for item in scoreArr:
        ballScores += str(item) + ' '
    for item in totalScoreArr:
        totalScores.append(str(item))
        totalScores.append(' | ')

    if lineCount - len(totalScoreArr) > 0:
        for i in range(lineCount-len(totalScoreArr)):
            totalScores.append(' | ')
            totalScores.append(' ')
    if totalScores[-1] == ' | ':
        totalScores.pop()

    for item in totalScores:
        totalScoresStr += str(item)

    print(ballScores)
    print(totalScoresStr)

    return


def extraRounds(strike, spare, playerScoreArr):
    total = 0
    if strike:
        for i in range(2):
            pin1 = int(input('Pins:'))
            while pin1 < 0 or pin1 > 10:
                pin1 = int(input('An invalid input. Pins:'))
            if pin1 != 10:
                pin2 = int(input('Pins:'))
                while pin1+pin2 > 10 or pin2 < 0:
                    pin2 = int(input('An invalid input. Pins:'))
                playerScoreArr.append(pin1)
                playerScoreArr.append(pin2)
                total += pin1+pin2
            else:
                playerScoreArr.append(pin1)
                total += pin1
        return total

    elif spare:
        pin1 = int(input('Pins:'))
        while pin1 < 0 or pin1 > 10:
            pin1 = int(input('An invalid input. Pins:'))
        total += pin1
        playerScoreArr.append(pin1)
        return total, playerScoreArr

    else:
        return


def game():
    # General Variables
    round = 0
    isStrikeA = False
    isSpareA = False
    isStrikeB = False
    isSpareB = False

    strikeHolderA = False
    spareHolderA = False
    strikeHolderB = False
    spareHolderB = False

    # Round Totals go to these
    player1ScoreHolder = []
    player2ScoreHolder = []

    # Individual pins go to these
    player1ScoreArr = []
    player2ScoreArr = []

    # Total score go to these
    player1TotalArr = []
    player2TotalArr = []

    # Game Start
    while round < 36:

        # If last round there was a strike/spare then make holders true
        # Will describe in detail later on
        if isStrikeA:
            strikeHolderA = True
            isStrikeA = False

        if isSpareA:
            spareHolderA = True
            isSpareA = False

        if isStrikeB:
            strikeHolderB = True
            isStrikeB = False

        if isSpareB:
            spareHolderB = True
            isSpareB = False

        # Start & ask pin

        print('Player A rolls...')
        pin1 = int(input('Pins:'))
        # Validate pin1
        while pin1 < 0 or pin1 > 10:
            pin1 = int(input('An invalid input. Pins:'))
        round+=1

        if isStrike(pin1):
            isStrikeA = True
            player1ScoreArr.append(pin1)
            pin2 = 0
            round+=1

        else:
            pin2 = int(input('Pins:'))
            # Validate pin2
            while pin1 + pin2 > 10 or pin2 < 0:
                pin2 = int(input('An invalid input. Pins:'))
            round+=1

            if isSpare(pin1, pin2):
                isSpareA = True

            player1ScoreArr.append(pin1)
            player1ScoreArr.append(pin2)

        # If holders are true, that means one round has passed and they go into calculation with the new pin values
        if strikeHolderA:
            total = calculateRoundTotalScore(
                strikeHolderA, spareHolderA, pin1, pin2)
            player1ScoreHolder.append(total)
            # If something is added to the round total list(holderLists), calculate total score will commence
            calculateTotalScore(player1ScoreHolder, player1TotalArr)
            strikeHolderA = False
        elif spareHolderA:
            total = calculateRoundTotalScore(
                strikeHolderA, spareHolderA, pin1, pin2)
            player1ScoreHolder.append(total)

            calculateTotalScore(player1ScoreHolder, player1TotalArr)
            spareHolderA = False

        # If there was a strike/spare then the round will pass
        # If not the score calculations will commence
        if not (isStrikeA or isSpareA):
            total = calculateRoundTotalScore(
                strikeHolderA, spareHolderA, pin1, pin2)
            player1ScoreHolder.append(total)

            calculateTotalScore(player1ScoreHolder, player1TotalArr)

        printScores(player1ScoreArr, player1TotalArr)
        print('Player B rolls...')
        pin1 = int(input('Pins:'))
        # Validate pin1
        while pin1 < 0 or pin1 > 10:
            pin1 = int(input('An invalid input. Pins:'))
        round+=1

        if isStrike(pin1):
            isStrikeB = True
            player2ScoreArr.append(pin1)
            pin2 = 0
            round+=1

        else:
            pin2 = int(input('Pins:'))
            # Validate pin2
            while pin1 + pin2 > 10 or pin2 < 0:
                pin2 = int(input('An invalid input. Pins:'))
            round+=1

            if isSpare(pin1, pin2):
                isSpareB = True

            player2ScoreArr.append(pin1)
            player2ScoreArr.append(pin2)

        if strikeHolderB:
            total = calculateRoundTotalScore(
                strikeHolderB, spareHolderB, pin1, pin2)
            player2ScoreHolder.append(total)

            calculateTotalScore(player2ScoreHolder, player2TotalArr)
            strikeHolderB = False
        elif spareHolderB:
            total = calculateRoundTotalScore(
                strikeHolderB, spareHolderB, pin1, pin2)
            player2ScoreHolder.append(total)

            calculateTotalScore(player2ScoreHolder, player2TotalArr)
            spareHolderB = False

        if not (isStrikeB or isSpareB):
            total = calculateRoundTotalScore(
                strikeHolderB, spareHolderB, pin1, pin2)
            player2ScoreHolder.append(total)

            calculateTotalScore(player2ScoreHolder, player2TotalArr)

        printScores(player2ScoreArr, player2TotalArr)

    if round ==36:
        total= extraRounds(isStrikeA, isSpareA, player1ScoreArr) 
        calculateTotalScore(player1ScoreArr, player1TotalArr)
        total= extraRounds(isStrikeB, isSpareB, player2ScoreArr) 
        calculateTotalScore(player2ScoreArr, player2TotalArr)

        
        if player1TotalArr[0]>player2TotalArr[0]:
            print("Winner is player A")
        else:
            print("Winner is player B")
        
        round+=4
   


def main():
    game()


if __name__ == '__main__':
    main()
