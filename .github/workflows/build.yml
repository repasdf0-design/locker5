#include <windows.h>
#include <string>

#define ID_EDIT 101
#define ID_BUTTON 102

HWND hEdit;

LRESULT CALLBACK WndProc(HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam) {
    switch (msg) {
    case WM_CREATE: {
        // Поле ввода
        hEdit = CreateWindowEx(0, L"EDIT", L"",
            WS_CHILD | WS_VISIBLE | WS_BORDER | ES_CENTER,
            0, 0, 300, 40, hwnd, (HMENU)ID_EDIT,
            ((LPCREATESTRUCT)lParam)->hInstance, NULL);

        // Кнопка "Войти"
        CreateWindowEx(0, L"BUTTON", L"Войти",
            WS_CHILD | WS_VISIBLE | BS_PUSHBUTTON,
            0, 0, 150, 40, hwnd, (HMENU)ID_BUTTON,
            ((LPCREATESTRUCT)lParam)->hInstance, NULL);

        // Шрифт крупнее
        HFONT hFont = CreateFontW(28, 0, 0, 0, FW_NORMAL, 0, 0, 0,
            DEFAULT_CHARSET, 0, 0, 0, 0, L"Segoe UI");
        SendMessage(hEdit, WM_SETFONT, (WPARAM)hFont, TRUE);
        break;
    }
    case WM_COMMAND:
        if (LOWORD(wParam) == ID_BUTTON) {
            wchar_t buf[32];
            GetWindowTextW(hEdit, buf, 32);
            if (wcscmp(buf, L"5559") == 0) {
                DestroyWindow(hwnd); // верный пароль — закрываемся
            } else {
                MessageBoxW(hwnd, L"Неверный пароль", L"Ошибка", MB_OK | MB_ICONERROR);
                SetWindowTextW(hEdit, L"");
            }
        }
        break;
    case WM_SIZE: {
        // Располагаем элементы по центру
        int w = LOWORD(lParam);
        int h = HIWORD(lParam);
        SetWindowPos(hEdit, NULL, w/2 - 150, h/2 - 60, 300, 40, SWP_NOZORDER);
        SetWindowPos(GetDlgItem(hwnd, ID_BUTTON), NULL,
                     w/2 - 75, h/2, 150, 40, SWP_NOZORDER);
        break;
    }
    case WM_CLOSE:
        // Разрешаем закрытие через крестик — это шутка, не локер
        DestroyWindow(hwnd);
        break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProcW(hwnd, msg, wParam, lParam);
    }
    return 0;
}

int WINAPI wWinMain(HINSTANCE hInst, HINSTANCE, PWSTR, int nCmdShow) {
    WNDCLASSW wc = {};
    wc.lpfnWndProc = WndProc;
    wc.hInstance = hInst;
    wc.lpszClassName = L"JokeLockerClass";
    wc.hbrBackground = CreateSolidBrush(RGB(20, 20, 30));
    wc.hCursor = LoadCursor(NULL, IDC_ARROW);
    RegisterClassW(&wc);

    // Полноэкранное окно без рамки
    int sw = GetSystemMetrics(SM_CXSCREEN);
    int sh = GetSystemMetrics(SM_CYSCREEN);

    HWND hwnd = CreateWindowExW(0, L"JokeLockerClass",
        L"Введите пароль",
        WS_POPUP | WS_VISIBLE,
        0, 0, sw, sh,
        NULL, NULL, hInst, NULL);

    ShowWindow(hwnd, nCmdShow);
    UpdateWindow(hwnd);

    MSG msg;
    while (GetMessage(&msg, NULL, 0, 0)) {
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    return 0;
}
