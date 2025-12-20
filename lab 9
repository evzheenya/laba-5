#include <stdio.h>     
#include <conio.h>      
#include<iostream>
#include <windows.h> 
int main() {
    setlocale(LC_ALL, "Russian");
    char key;   
    printf("Консольное фортепиано (a-j). 'q' для выхода.\n");   
    while (1) {    
        if (_kbhit()) {    
            key = _getch(); 
            if (key == 'q') {      
                printf("\nВыход из программы.\n");      
                break;
            }
            int freq = 0;   
            switch (key) {    
            case 'a': std::cout << "Do "; freq = Beep(262, 500); break; 
            case '1': std::cout << "Do "; freq = Beep(262, 1000); break; 
            case 's': std::cout << "Re "; freq = Beep(294,1000); break; 
            case 'd': std::cout << "Mi "; freq = Beep(330,1500); break; 
            case 'f': std::cout << "Fa "; freq = Beep(349,1200); break; 
            case '2': std::cout << "Fa "; freq = Beep(349,1500); break; 
            case 'g': std::cout << "Sol "; freq = Beep(392,300); break;
            default: continue; 
            }
        }
    }
    return 0;
}
