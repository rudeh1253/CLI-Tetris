#include <iostream>
#include <conio.h>
#include <Windows.h>
#include <stdlib.h>
#include <process.h>
#include <time.h>
#include <thread>

#define WIDTH    10
#define HEIGHT    20

#define UP    72
#define DOWN    80
#define LEFT    75
#define RIGHT    77
#define ARROW_PREVIOUS    224

#define CTRABLE    2
#define TRUE    1
#define FALSE    0

#define POINTX    ((x * 2) + (point.x * 2))
#define POINTY    (y + point.y)

char finish;
char arrived;
char map[WIDTH + 4][HEIGHT + 2];
char click_down = FALSE;
char can_clear = FALSE;
char mino[7][4][4][4] = {
	{ // L
		0, 0, 1, 0,
		0, 0, 1, 0,
		0, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 0, 0,
		0, 1, 1, 1,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 1, 0,
		0, 1, 0, 0,
		0, 1, 0, 0,

		0, 0, 0, 0,
		1, 1, 1, 0,
		0, 0, 1, 0,
		0, 0, 0, 0		
	},
	{ // J
		0, 0, 0, 0,
		0, 1, 1, 0,
		0, 0, 1, 0,
		0, 0, 1, 0,

		0, 0, 0, 0,
		0, 0, 1, 0,
		1, 1, 1, 0,
		0, 0, 0, 0,

		0, 1, 0, 0,
		0, 1, 0, 0,
		0, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 1, 1,
		0, 1, 0, 0,
		0, 0, 0, 0
	},
	{ // Z
		0, 0, 0, 0,
		0, 0, 1, 0,
		0, 1, 1, 0,
		0, 1, 0, 0,

		0, 0, 0, 0,
		1, 1, 0, 0,
		0, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 0, 1, 0,
		0, 1, 1, 0,
		0, 1, 0, 0,

		0, 0, 0, 0,
		1, 1, 0, 0,
		0, 1, 1, 0,
		0, 0, 0, 0
	},
	{ // S
		0, 0, 0, 0,
		0, 1, 0, 0,
		0, 1, 1, 0,
		0, 0, 1, 0,

		0, 0, 0, 0,
		0, 1, 1, 0,
		1, 1, 0, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 0, 0,
		0, 1, 1, 0,
		0, 0, 1, 0,

		0, 0, 0, 0,
		0, 1, 1, 0,
		1, 1, 0, 0,
		0, 0, 0, 0
	},
	{ // I
		0, 1, 0, 0,
		0, 1, 0, 0,
		0, 1, 0, 0,
		0, 1, 0, 0,

		0, 0, 0, 0,
		1, 1, 1, 1,
		0, 0, 0, 0,
		0, 0, 0, 0,

		0, 0, 1, 0,
		0, 0, 1, 0,
		0, 0, 1, 0,
		0, 0, 1, 0,

		0, 0, 0, 0,
		1, 1, 1, 1,
		0, 0, 0, 0,
		0, 0, 0, 0
	},
	{ // O
		0, 0, 0, 0,
		0, 1, 1, 0,
		0, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 1, 0,
		0, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 1, 0,
		0, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 1, 0,
		0, 1, 1, 0,
		0, 0, 0, 0
	},
	{ // T
		0, 0, 0, 0,
		0, 1, 0, 0,
		1, 1, 0, 0,
		0, 1, 0, 0,

		0, 0, 0, 0,
		0, 1, 0, 0,
		1, 1, 1, 0,
		0, 0, 0, 0,

		0, 0, 0, 0,
		0, 1, 0, 0,
		0, 1, 1, 0,
		0, 1, 0, 0,

		0, 0, 0, 0,
		0, 0, 0, 0,
		1, 1, 1, 0,
		0, 1, 0, 0
	}
};
int speed;
int mino_pose;
int get_num;
int keypress = 0;
int score = 0;
const int LENGTH = 4; // 미노의 최대 길이

typedef struct _Point {
	int x;
	int y;
} Point;
Point point;

void gotoxy(short x, short y) {
	COORD Pos;
	Pos.X = x;
	Pos.Y = y;
	SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), Pos);
}

void start_map() {
	for (int i = 0; i < sizeof(map) / sizeof(map[0]); i++)
		for (int j = 0; j < sizeof(map[0]); j++)
			map[i][j] = 0;
	for (int y = 0; y < HEIGHT + 1; y++) {
		map[0][y] = 1; map[1][y] = 1;
		map[WIDTH + 2][y] = 1; map[WIDTH + 3][y] = 1;
	}
	for (int x = 0; x < WIDTH + 4; x++) {
		map[x][HEIGHT] = 1;
		map[x][HEIGHT + 1] = 1;
	}
	for (int x = 0; x < WIDTH + 4; x++)
		for (int y = 1; y < HEIGHT + 2; y++) {
			if (map[x][y] == TRUE) {
				gotoxy(x * 2, y);
				std::cout << "㎡";
			}
		}
}

void get_mino() {
	srand((unsigned int)time(NULL));
	get_num = rand() % 7;
    mino_pose = 0;
	point.x = 4;
	point.y = 0;

	for (int x = 0; x < LENGTH; x++)
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE) {
				map[x + point.x][y] = CTRABLE;
				gotoxy(POINTX, POINTY);
				std::cout << "■";
			}
}

void convert_CTRABLE_to_wall() {
	for (int x = 0; x < LENGTH; x++)
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE)
				if (map[x + point.x][y + point.y] == CTRABLE)
					map[x + point.x][y + point.y] = TRUE;
}

void mino_fall() {
	for (int y = LENGTH - 1; y >= 0; y--) // 이동할 곳에 벽이 있는지 없는지 판단
		for (int x = 0; x < LENGTH; x++) {
			if (mino[get_num][mino_pose % 4][x][y] == TRUE)
				if (map[x + point.x][y + point.y + 1] == TRUE) {
					arrived = TRUE; // 한 칸 아래에 벽이 있어서 떨어지지 못하기 때문에 미노가 떨어질 수 있는 곳까지 떨어졌다고 판단
					convert_CTRABLE_to_wall();
					return;
				}
		}
	
	for (int y = LENGTH - 1; y >= 0; y--)
		for (int x = 0; x < LENGTH; x++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE) {
				map[x + point.x][y + point.y + 1] = CTRABLE;
				map[x + point.x][y + point.y] = FALSE;
				gotoxy(POINTX, POINTY + 1);
				std::cout << "■";
				gotoxy(POINTX, POINTY);
				std::cout << "  ";
			}
	point.y++; // 미노의 좌표가 한 칸 떨어짐
}

void go_left() {
	for (int x = 0; x < LENGTH; x++)
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE)
				if (map[x + point.x - 1][y + point.y] == TRUE) return;

	for (int x = 0; x < LENGTH; x++)
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE) {
				map[x + point.x - 1][y + point.y] = CTRABLE;
				map[x + point.x][y + point.y] = FALSE;
				gotoxy(POINTX - 2, POINTY);
				std::cout << "■";
				gotoxy(POINTX, POINTY);
				std::cout << "  ";
			}
	point.x--;
}

void go_right() {
	for (int x = LENGTH - 1; x >= 0; x--)
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE)
				if (map[x + point.x + 1][y + point.y] == TRUE) return;

	for (int x = LENGTH - 1; x >= 0; x--)
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][mino_pose % 4][x][y] == TRUE) {
				map[x + point.x + 1][y + point.y] = CTRABLE;
				map[x + point.x][y + point.y] = FALSE;
				gotoxy(POINTX + 2, POINTY);
				std::cout << "■";
				gotoxy(POINTX, POINTY);
				std::cout << "  ";
			}
	point.x++;
}

void spin() {
	char spin_possible = TRUE;

	for (int x = 0; x < LENGTH; x++) {
		for (int y = 0; y < LENGTH; y++)
			if (mino[get_num][(mino_pose + 1) % 4][x][y] == TRUE)
				if (map[x + point.x][y + point.y] == TRUE) {
					spin_possible = FALSE;
					break;
				}
		if (spin_possible == FALSE) break;
	}
	if (spin_possible) {
		for (int x = 0; x < LENGTH; x++) 
			for (int y = 0; y < LENGTH; y++)
				if (mino[get_num][mino_pose % 4][x][y] == TRUE) {
					map[x + point.x][y + point.y] = FALSE;
					gotoxy(POINTX, POINTY);
					std::cout << "  ";
				}
		mino_pose++;
		for (int x = 0; x < LENGTH; x++)
			for (int y = 0; y < LENGTH; y++)
				if (mino[get_num][mino_pose % 4][x][y] == TRUE) {
					map[x + point.x][y + point.y] = CTRABLE;
					gotoxy(POINTX, POINTY);
					std::cout << "■";
				}
		return;
	}
}

void scoring() {
	gotoxy((WIDTH + 8) * 2, 4);
	std::cout << "없앤 라인 수 : " << score;
}

void clear(int stairs) {
	for (int x = 2; x < WIDTH + 2; x++) {
		map[x][stairs] = 0;
		gotoxy(x * 2, stairs);
		std::cout << "  ";
	}
	for (int y = stairs - 1; y >= 0; y--) {
		for (int x = 2; x < WIDTH + 2; x++) {
			if (map[x][y] == TRUE) {
				map[x][y + 1] = TRUE;
				map[x][y] = FALSE;
				gotoxy(x * 2, y + 1);
				std::cout << "■";
				gotoxy(x * 2, y);
				std::cout << "  ";
			}
		}
	}
}

void check() {
	char complete;
	int idx = 0;
	int count;

	for (int y = HEIGHT - 1; y >= 0; y--) {
		count = 0;
		for (int x = 2; x < WIDTH + 2; x++)
			if (map[x][y] == TRUE) {
				count++;
			}
		if (count == 10) { clear(y); score++; scoring(); }
	}
}

void speed_up() {
	while (speed <= 10) {
		Sleep(10000); speed++;
	}
}

void game_over() {

}

void button() {
	while (!finish) {
		keypress = _getch();
		if (keypress == ARROW_PREVIOUS)
			keypress = _getch();
	}
}

int main() {
	finish = FALSE;
	// system("color F0");

	while (1) {
		int select;
		std::cout << "속도\n1. 1배\n2. 2배\n4. 3배\n0. 설명" << std::endl; std::cin >> select;
		if (select == 1 || select == 2 || select == 3) {
			speed = select;
			break;
		}
		else if (select == 0)
			std::cout << "화살표로 조작\n왼쪽키, 오른쪽키, 아래키는 이동, 위 키는 회전(자연은 시계방향)" << std::endl;
		else
			std::cout << "잘못된 입력\n" << std::endl;
	}

	arrived = TRUE;
	system("cls");
	start_map();
	clock_t ck = clock();
	clock_t interval;
	std::thread td(button);
	td.detach();
	std::thread spdup(speed_up);
	spdup.detach();
	
	while (!finish) { // 게임 루프 시작
		if (arrived) {
			get_mino();
			arrived = FALSE;
		}
		
		switch (keypress) {
		case LEFT: go_left(); keypress = FALSE; break;
		case RIGHT: go_right(); keypress = FALSE; break;
		case DOWN: mino_fall(); keypress = FALSE; break;
		case UP: spin(); keypress = FALSE; break;
		default: break;
		}
		
		interval = clock();
		if (interval - ck >= 1000 / speed) {
			mino_fall();
			ck = clock();
		}

		check();

		Sleep(1);
	}
	return 0;
}
