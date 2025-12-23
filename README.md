#include <windows.h>

LRESULT CALLBACK WndProc(HWND, UINT, WPARAM, LPARAM);

int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance,
                   PSTR szCmdLine, int iCmdShow) {
    static TCHAR szAppName[] = TEXT("EllipseExample");
    HWND hwnd;
    MSG msg;
    WNDCLASS wndclass;

    wndclass.style = CS_HREDRAW | CS_VREDRAW;
    wndclass.lpfnWndProc = WndProc;
    wndclass.cbClsExtra = 0;
    wndclass.cbWndExtra = 0;
    wndclass.hInstance = hInstance;
    wndclass.hIcon = LoadIcon(NULL, IDI_APPLICATION);
    wndclass.hCursor = LoadCursor(NULL, IDC_ARROW);
    wndclass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);
    wndclass.lpszMenuName = NULL;
    wndclass.lpszClassName = szAppName;

    if (!RegisterClass(&wndclass)) {
        MessageBox(NULL, TEXT("Ошибка регистрации класса!"),
                   szAppName, MB_ICONERROR);
        return 0;
    }

    hwnd = CreateWindow(szAppName, TEXT("Желтый эллипс с красной границей"),
                        WS_OVERLAPPEDWINDOW,
                        CW_USEDEFAULT, CW_USEDEFAULT,
                        400, 300,
                        NULL, NULL, hInstance, NULL);

    ShowWindow(hwnd, iCmdShow);
    UpdateWindow(hwnd);

    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    return msg.wParam;
}

LRESULT CALLBACK WndProc(HWND hwnd, UINT message, WPARAM wParam, LPARAM lParam) {
    HDC hdc;
    PAINTSTRUCT ps;
    HBRUSH hBrush, hOldBrush;
    HPEN hPen, hOldPen;
    RECT rect;

    switch (message) {
    case WM_PAINT:
        hdc = BeginPaint(hwnd, &ps);
        
        // Получаем размеры клиентской области
        GetClientRect(hwnd, &rect);
        
        // Создаем желтую кисть для заливки
        hBrush = CreateSolidBrush(RGB(255, 255, 0)); // Желтый цвет
        hOldBrush = (HBRUSH)SelectObject(hdc, hBrush);
        
        // Создаем красное перо для границы
        hPen = CreatePen(PS_SOLID, 2, RGB(255, 0, 0)); // Красная граница толщиной 2 пикселя
        hOldPen = (HPEN)SelectObject(hdc, hPen);
        
        // Рисуем эллипс, который займет 80% от размеров окна
        Ellipse(hdc,
                rect.right * 0.1,   // левая граница (10% от ширины)
                rect.bottom * 0.1,  // верхняя граница (10% от высоты)
                rect.right * 0.9,   // правая граница (90% от ширины)
                rect.bottom * 0.9); // нижняя граница (90% от высоты)
        
        // Восстанавливаем оригинальные объекты
        SelectObject(hdc, hOldBrush);
        SelectObject(hdc, hOldPen);
        
        // Удаляем созданные GDI-объекты
        DeleteObject(hBrush);
        DeleteObject(hPen);
        
        EndPaint(hwnd, &ps);
        return 0;
        
    case WM_DESTROY:
        PostQuitMessage(0);
        return 0;
    }
    
    return DefWindowProc(hwnd, message, wParam, lParam);
}
