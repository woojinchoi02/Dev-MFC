# Dev, MFC
-------
## Dev-CPP 사각형 큐브 대신 다른 도형 넣기
Dev-CPP를 이용하여 사각형 큐브을 삼각뿔로 변형하고 회전시켰습니다.
![과제 큐브](https://github.com/woojinchoi02/Graphics-dev-mfc/assets/162526228/49990532-fb07-4ffb-93b6-75095591de48)












### Dev코드
      #include <windows.h>
      #include <gl/gl.h>
      #include <math.h>
      
      /**************************
       * Function Declarations
       *
       **************************/
      
      LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam);
      void EnableOpenGL(HWND hWnd, HDC *hDC, HGLRC *hRC);
      void DisableOpenGL(HWND hWnd, HDC hDC, HGLRC hRC);
      
      /**************************
       * WinMain
       *
       **************************/
      
      int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpCmdLine, int iCmdShow)
      {
          WNDCLASS wc;
          HWND hWnd;
          HDC hDC;
          HGLRC hRC;
          MSG msg;
          BOOL bQuit = FALSE;
          float theta = 0.0f;
      
          /* register window class */
          wc.style = CS_OWNDC;
          wc.lpfnWndProc = WndProc;
          wc.cbClsExtra = 0;
          wc.cbWndExtra = 0;
          wc.hInstance = hInstance;
          wc.hIcon = LoadIcon(NULL, IDI_APPLICATION);
          wc.hCursor = LoadCursor(NULL, IDC_ARROW);
          wc.hbrBackground = (HBRUSH)GetStockObject(BLACK_BRUSH);
          wc.lpszMenuName = NULL;
          wc.lpszClassName = "GLSample";
          RegisterClass(&wc);
      
          /* create main window */
          hWnd = CreateWindow(
              "GLSample", "OpenGL Sample",
              WS_CAPTION | WS_POPUPWINDOW | WS_VISIBLE,
              0, 0, 512, 512,
              NULL, NULL, hInstance, NULL);
      
          /* enable OpenGL for the window */
          EnableOpenGL(hWnd, &hDC, &hRC);
      
          /* program main loop */
          while (!bQuit)
          {
              /* check for messages */
              if (PeekMessage(&msg, NULL, 0, 0, PM_REMOVE))
              {
                  /* handle or dispatch messages */
                  if (msg.message == WM_QUIT)
                  {
                      bQuit = TRUE;
                  }
                  else
                  {
                      TranslateMessage(&msg);
                      DispatchMessage(&msg);
                  }
              }
              else
              {
                  /* OpenGL animation code goes here */
      
                  glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
                  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
      
                  glPushMatrix();
                  glRotatef(theta, 0.0f, 1.0f, 0.0f); // y축을 기준으로 회전
                  glRotatef(theta, 0.0f, 0.0f, 1.0f); // z축을 기준으로 회전
      
                  glBegin(GL_TRIANGLES);
      
                  // 첫 번째 삼각형
                  glColor3f(1.0f, 0.0f, 0.0f); glVertex3f(0.0f, 1.0f, 0.0f);  // 꼭짓점
                  glColor3f(0.0f, 1.0f, 0.0f); glVertex3f(-0.5f, 0.0f, 0.5f); // 왼쪽 아래
                  glColor3f(0.0f, 0.0f, 1.0f); glVertex3f(0.5f, 0.0f, 0.5f);  // 오른쪽 아래
      
                  // 두 번째 삼각형
                  glColor3f(1.0f, 0.0f, 0.0f); glVertex3f(0.0f, 1.0f, 0.0f);  // 꼭짓점
                  glColor3f(0.0f, 0.0f, 1.0f); glVertex3f(0.5f, 0.0f, 0.5f);  // 오른쪽 아래
                  glColor3f(1.0f, 1.0f, 0.0f); glVertex3f(0.0f, 0.0f, -0.5f); // 뒤쪽 아래
      
                  // 세 번째 삼각형
                  glColor3f(1.0f, 0.0f, 0.0f); glVertex3f(0.0f, 1.0f, 0.0f);  // 꼭짓점
                  glColor3f(1.0f, 1.0f, 0.0f); glVertex3f(0.0f, 0.0f, -0.5); // 뒤쪽 아래
                  glColor3f(0.0f, 1.0f, 0.0f); glVertex3f(-0.5f, 0.0f, 0.5f); // 왼쪽 아래
      
                  // 네 번째 삼각형 (밑면)
                  glColor3f(0.0f, 1.0f, 0.0f); glVertex3f(-0.5f, 0.0f, 0.5f); // 왼쪽 아래
                  glColor3f(0.0f, 0.0f, 1.0f); glVertex3f(0.5f, 0.0f, 0.5f);  // 오른쪽 아래
                  glColor3f(1.0f, 1.0f, 0.0f); glVertex3f(0.0f, 0.0f, -0.5f); // 뒤쪽 아래
      
                  glEnd();
      
                  // 삼각형 윤곽선
                  glLineWidth(2.0f); // 윤곽선 굵기 설정
                  glColor3f(1.0f, 1.0f, 1.0f); // 흰색 윤곽선 색상 설정
      
                  glBegin(GL_LINE_LOOP);
      
                  // 첫 번째 삼각형 윤곽선
                  glVertex3f(0.0f, 1.0f, 0.0f);
                  glVertex3f(-0.5f, 0.0f, 0.5f);
                  glVertex3f(0.5f, 0.0f, 0.5f);
      
                  glEnd();
      
                  glBegin(GL_LINE_LOOP);
      
                  // 두 번째 삼각형 윤곽선
                  glVertex3f(0.0f, 1.0f, 0.0f);
                  glVertex3f(0.5f, 0.0f, 0.5f);
                  glVertex3f(0.0f, 0.0f, -0.5f);
      
                  glEnd();
      
                  glBegin(GL_LINE_LOOP);
      
                  // 세 번째 삼각형 윤곽선
                  glVertex3f(0.0f, 1.0f, 0.0f);
                  glVertex3f(0.0f, 0.0f, -0.5f);
                  glVertex3f(-0.5f, 0.0f, 0.5f);
      
                  glEnd();
      
                  glBegin(GL_LINE_LOOP);
      
                  // 네 번째 삼각형 (밑면) 윤곽선
                  glVertex3f(-0.5f, 0.0f, 0.5f);
                  glVertex3f(0.5f, 0.0f, 0.5f);
                  glVertex3f(0.0f, 0.0f, -0.5f);
      
                  glEnd();
      
                  glPopMatrix();
      
                  SwapBuffers(hDC);
      
                  theta += 0.02f; // 반시계 방향으로 회전
                  //Sleep(10);
              }
          }
      
          /* shutdown OpenGL */
          DisableOpenGL(hWnd, hDC, hRC);
      
          /* destroy the window explicitly */
          DestroyWindow(hWnd);
      
          return msg.wParam;
      }
      
      /********************
       * Window Procedure
       *
       ********************/
      
      LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
      {
          switch (message)
          {
          case WM_CREATE:
              return 0;
          case WM_CLOSE:
              PostQuitMessage(0);
              return 0;
          case WM_DESTROY:
              return 0;
          case WM_KEYDOWN:
              switch (wParam)
              {
              case VK_ESCAPE:
                  PostQuitMessage(0);
                  return 0;
              }
              return 0;
          default:
              return DefWindowProc(hWnd, message, wParam, lParam);
          }
      }
      
      /*******************
       * Enable OpenGL
       *
       *******************/
      
      void EnableOpenGL(HWND hWnd, HDC *hDC, HGLRC *hRC)
      {
          PIXELFORMATDESCRIPTOR pfd;
          int iFormat;
      
          /* get the device context (DC) */
          *hDC = GetDC(hWnd);
      
          /* set the pixel format for the DC */
          ZeroMemory(&pfd, sizeof(pfd));
          pfd.nSize = sizeof(pfd);
          pfd.nVersion = 1;
          pfd.dwFlags = PFD_DRAW_TO_WINDOW |
              PFD_SUPPORT_OPENGL | PFD_DOUBLEBUFFER;
          pfd.iPixelType = PFD_TYPE_RGBA;
          pfd.cColorBits = 24;
          pfd.cDepthBits = 16;
          pfd.iLayerType = PFD_MAIN_PLANE;
          iFormat = ChoosePixelFormat(*hDC, &pfd);
          SetPixelFormat(*hDC, iFormat, &pfd);
      
          /* create and enable the render context (RC) */
          *hRC = wglCreateContext(*hDC);
          wglMakeCurrent(*hDC, *hRC);
      
          glEnable(GL_DEPTH_TEST); // Enable depth testing
      }
      
      /******************
       * Disable OpenGL
       *
       ******************/
      
      void DisableOpenGL(HWND hWnd, HDC hDC, HGLRC hRC)
      {
          wglMakeCurrent(NULL, NULL);
          wglDeleteContext(hRC);
          ReleaseDC(hWnd, hDC);
      }


