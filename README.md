# Dev, MFC
-------
## Dev-CPP 사각형 큐브 대신 다른 도형 넣기
Dev-CPP를 이용하여 사각형 큐브을 삼각뿔로 변형하고 회전시켰습니다.
![과제1](https://github.com/woojinchoi02/Graphics-dev-mfc/assets/162526228/bedc2efe-3655-4d46-ae99-364f1e68ccae)
>코드는 아래 있습니다.

## MFC에서 사각형, 타원, 글씨에 다른 도형 추가하기
MFC를 이용하여 안동하면 떠오르는 닭을 만들어보았습니다.
![과제2](https://github.com/woojinchoi02/Graphics-dev-mfc/assets/162526228/55b4ea74-aba5-4564-8694-a41d3784fd13)
>>코드는 아래 있습니다.

------
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
------
### MFC코드

                        // MFCApplication1View.cpp: CMFCApplication1View 클래스의 구현
                        //
                        
                        #include "pch.h"
                        #include "framework.h"
                        // SHARED_HANDLERS는 미리 보기, 축소판 그림 및 검색 필터 처리기를 구현하는 ATL 프로젝트에서 정의할 수 있으며
                        // 해당 프로젝트와 문서 코드를 공유하도록 해 줍니다.
                        #ifndef SHARED_HANDLERS
                        #include "MFCApplication1.h"
                        #endif
                        
                        #include "MFCApplication1Doc.h"
                        #include "MFCApplication1View.h"
                        
                        #ifdef _DEBUG
                        #define new DEBUG_NEW
                        #endif
                        
                        
                        // CMFCApplication1View
                        
                        IMPLEMENT_DYNCREATE(CMFCApplication1View, CView)
                        
                        BEGIN_MESSAGE_MAP(CMFCApplication1View, CView)
                        	// 표준 인쇄 명령입니다.
                        	ON_COMMAND(ID_FILE_PRINT, &CView::OnFilePrint)
                        	ON_COMMAND(ID_FILE_PRINT_DIRECT, &CView::OnFilePrint)
                        	ON_COMMAND(ID_FILE_PRINT_PREVIEW, &CView::OnFilePrintPreview)
                        	ON_WM_MOUSEMOVE()
                        END_MESSAGE_MAP()
                        
                        // CMFCApplication1View 생성/소멸
                        
                        CMFCApplication1View::CMFCApplication1View() noexcept
                        {
                        	// TODO: 여기에 생성 코드를 추가합니다.
                        
                        }
                        
                        CMFCApplication1View::~CMFCApplication1View()
                        {
                        }
                        
                        BOOL CMFCApplication1View::PreCreateWindow(CREATESTRUCT& cs)
                        {
                        	// TODO: CREATESTRUCT cs를 수정하여 여기에서
                        	//  Window 클래스 또는 스타일을 수정합니다.
                        
                        	return CView::PreCreateWindow(cs);
                        }
                        
                        // CMFCApplication1View 그리기
                        CPoint pnt;
                        void CMFCApplication1View::OnDraw(CDC* pDC)
                        {
                        	CMFCApplication1Doc* pDoc = GetDocument();
                        	ASSERT_VALID(pDoc);
                        	if (!pDoc)
                        		return;
                        	POINT verticeshead1[] = { 
                        		{ pnt.x, pnt.y-130 },
                        		{ pnt.x , pnt.y - 90 },
                        		{ pnt.x + 40, pnt.y -110 } };
                        	POINT verticeshead2[] = {
                        	{ pnt.x, pnt.y - 100 },
                        	{ pnt.x , pnt.y - 80 },
                        	{ pnt.x + 20, pnt.y - 90 } };
                        	POINT verticesmouse[] = {
                        	{ pnt.x-20, pnt.y },
                        	{ pnt.x+20, pnt.y},
                        	{ pnt.x , pnt.y + 40 } };
                        
                        	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.
                        	pDC->Rectangle(pnt.x - 100, pnt.y - 100, pnt.x + 100, pnt.y + 100);
                        	//------------------------------------눈
                        	CBrush eyeBrush(RGB(0, 0, 0));
                        	CPen eyePen(PS_SOLID, 1, RGB(0, 0, 0));
                        	pDC->SelectObject(&eyeBrush);
                        	pDC->SelectObject(&eyePen);
                        	pDC->Ellipse(pnt.x - 40, pnt.y - 60, pnt.x -20, pnt.y-25);
                        	pDC->Ellipse(pnt.x + 40, pnt.y - 60, pnt.x + 20, pnt.y - 25);
                        	//----------------------------------------------------머리
                        	CBrush headBrush(RGB(255, 0, 0));
                        	CPen headPen(PS_SOLID, 1, RGB(255, 0, 0));
                        	pDC->SelectObject(&headBrush);
                        	pDC->SelectObject(&headPen);
                        	pDC->Polygon(verticeshead1,3);
                        	pDC->Polygon(verticeshead2, 3);
                        	//------------------------------------------입
                        	CBrush mouseBrush(RGB(255, 255, 0));
                        	CPen mousePen(PS_SOLID, 1, RGB(255, 255, 0));
                        	pDC->SelectObject(&mouseBrush);
                        	pDC->SelectObject(&mousePen);
                        	pDC->Polygon(verticesmouse, 3);
                        	//-----------------------------------글씨
                        	pDC->SetTextColor(RGB(255, 0, 0)); // 텍스트 색상 빨간색
                        	pDC->TextOutW(pnt.x -40, pnt.y-130, L"닭");
                        	pDC->TextOutW(pnt.x - 50, pnt.y + 10, L"///");
                        	pDC->TextOutW(pnt.x + 50, pnt.y + 10, L"///");
                        }
                        
                        
                        // CMFCApplication1View 인쇄
                        
                        BOOL CMFCApplication1View::OnPreparePrinting(CPrintInfo* pInfo)
                        {
                        	// 기본적인 준비
                        	return DoPreparePrinting(pInfo);
                        }
                        
                        void CMFCApplication1View::OnBeginPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
                        {
                        	// TODO: 인쇄하기 전에 추가 초기화 작업을 추가합니다.
                        }
                        
                        void CMFCApplication1View::OnEndPrinting(CDC* /*pDC*/, CPrintInfo* /*pInfo*/)
                        {
                        	// TODO: 인쇄 후 정리 작업을 추가합니다.
                        }
                        
                        
                        // CMFCApplication1View 진단
                        
                        #ifdef _DEBUG
                        void CMFCApplication1View::AssertValid() const
                        {
                        	CView::AssertValid();
                        }
                        
                        void CMFCApplication1View::Dump(CDumpContext& dc) const
                        {
                        	CView::Dump(dc);
                        }
                        
                        CMFCApplication1Doc* CMFCApplication1View::GetDocument() const // 디버그되지 않은 버전은 인라인으로 지정됩니다.
                        {
                        	ASSERT(m_pDocument->IsKindOf(RUNTIME_CLASS(CMFCApplication1Doc)));
                        	return (CMFCApplication1Doc*)m_pDocument;
                        }
                        #endif //_DEBUG
                        
                        
                        // CMFCApplication1View 메시지 처리기
                        
                        
                        void CMFCApplication1View::OnMouseMove(UINT nFlags, CPoint point)
                        {
                        	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.
                        	pnt = point; // quiz2-2
                        	Invalidate(true); // quiz2-2
                        	CView::OnMouseMove(nFlags, point);
                        }


