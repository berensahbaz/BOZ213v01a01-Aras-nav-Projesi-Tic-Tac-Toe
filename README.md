# BOZ213v01a01-Aras-nav-Projesi-Tic-Tac-Toe
# BEREN ŞAHBAZ 24040089 TİC TAC TOE  BÖTE 2.SINIF ARASINAV PROJESİ



# Tic-Tac-Toe (Kolay sürüm)
# Oyuncu: 'O'     Bilgisayar: 'X'
# Kurallar: Bilgisayar merkeze 'X' koyarak başlar. Oyuncu 1..9 arası boş bir kare seçer.

from random import randrange

# --- Yardımcılar ---

def display_board(board):
    """Tahtayı örnektekiyle birebir çerçeveyle yazdırır."""
    horiz = "+-------+-------+-------+"
    for r in range(3):
        print(horiz)
        print("|       |       |       |")
        print("|", end="")
        for c in range(3):
            cell = str(board[r][c]).center(7)
            print(cell + "|", end="")
        print()
        print("|       |       |       |")
    print(horiz)

def make_list_of_free_fields(board):
    """Boş karelerin (satır, sütun) ikililerini döndürür."""
    free = []
    for r in range(3):
        for c in range(3):
            if isinstance(board[r][c], int):  # hâlâ numara ise boştur
                free.append((r, c))
    return free

def victory_for(board, sign):
    """Belirtilen işaret ( 'X' ya da 'O' ) kazandı mı?"""
    lines = []

    # Satırlar ve sütunlar
    for i in range(3):
        lines.append([board[i][0], board[i][1], board[i][2]])
        lines.append([board[0][i], board[1][i], board[2][i]])

    # Çaprazlar
    lines.append([board[0][0], board[1][1], board[2][2]])
    lines.append([board[0][2], board[1][1], board[2][0]])

    target = [sign, sign, sign]
    return any(line == target for line in lines)

def enter_move(board):
    """Kullanıcıdan geçerli bir hamle alır ve tahtaya 'O' koyar."""
    free = make_list_of_free_fields(board)
    free_nums = {board[r][c] for (r, c) in free}  # boş kare numaraları

    while True:
        try:
            move = int(input("Hamleni yap: "))
        except ValueError:
            print("Lütfen 1 ile 9 arasında bir sayı gir.")
            continue

        if move < 1 or move > 9:
            print("Aralık dışı. 1..9 olmalı.")
            continue
        if move not in free_nums:
            print("Bu kare dolu. Başka bir kare seç.")
            continue

        # numaradan (r, c) hesapla
        move -= 1
        r, c = divmod(move, 3)
        if isinstance(board[r][c], int):
            board[r][c] = 'O'
            break

def draw_move(board):
    """Bilgisayar boş karelerden rastgele birine 'X' koyar."""
    free = make_list_of_free_fields(board)
    if not free:
        return
    r, c = free[randrange(len(free))]
    board[r][c] = 'X'

# --- Oyun döngüsü ---

# Tahtayı 1..9 ile başlat, merkeze X koy
board = [[1, 2, 3],
         [4, 5, 6],
         [7, 8, 9]]
board[1][1] = 'X'  # bilgisayar ilk hamlesi

while True:
    display_board(board)

    # Oyuncu hamlesi
    enter_move(board)
    display_board(board)
    if victory_for(board, 'O'):
        print("Kazandın!")
        break
    if not make_list_of_free_fields(board):
        print("Berabere!")
        break

    # Bilgisayar hamlesi
    draw_move(board)
    if victory_for(board, 'X'):
        display_board(board)
        print("Bilgisayar kazandı!")
        break
    if not make_list_of_free_fields(board):
        display_board(board)
        print("Berabere!")
        break
