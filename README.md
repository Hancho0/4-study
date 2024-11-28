# 4주차 Study [ 2024-11-25 ~ 2024-11-29 ]

**[ 목표 ]**
- 이번주 목표 :
-----

**[ 진행 사항 ]**

MFC 프로그램은 SDI(Single Document Interface) 와 MDI(Multiple Document Interface)의 두 가지 형태로 나눌 수 있다.

SDI는 한 개의 도큐먼트 프레임 윈도우만을 사용하는 **단일 문서 기반 프로그램**
MDI는 같은 애플리케이션 인스턴스 안에 여러 개의 도큐먼트 프레임 윈도우를 사용할 수 있는 **여러 문서 기반 프로그램**

[ SDI 애플리케이션의 구조 ]<br>
기본적으로 네 개의 클래스가 있다.

CWinApp 파생 클래스 : 애플리케이션 전체를 나타내는 클래스<br>
CFrameWnd 파생 클래스 : 애플리케이션에서 메뉴와 상태 표시줄, 도구바를 포함한 외부 프레임을 나타내는 클래스<br>
CView 파생 클래스 : 애플리케이션의 클라이언트 또는 작업 영역을 나타내는 클래스<br>
CDocument 파생 클래스 : 애플리케이션 내부에서 데이터를 읽고, 저장하는 기능을 가진 클래스<br>

SDI 애플리케이션은 CFrameWnd와 CView 파생 클래스, CDocument 파생 클래스가 하나의 템플릿(Template)으로 구성된다. 그래서 단일 템플릿 애플리케이션이라고도 한다.

[ MDI 애플리케이션의 구조 ]<br>
기본적으로 다섯 개의 클래스가 있다.

CWinApp 파생 클래스 : 애플리케이션 전체를 나타내는 클래스<br>
CMDIFrameWnd 파생 클래스 : 애플리케이션에서 메뉴와 상태 표시줄, 도구바를 포함한 외부 프레임을 나타내는 클래스<br>
CMDIChildWnd 파생 클래스 : 애플리케이션에서 자식 윈도우의 외부 프레임을 나타내는 클래스<br>
CView 파생 클래스 : 애플리케이션에서 자식 윈도우의 클라이언트 또는 작업 영역을 나타내는 클래스<br>
CDocument 파생 클래스 : 애플리케이션 내부에서 데이터를 읽고, 저장하는 기능을 가진 클래스<br>

MDI 애플리케이션은 CMDIChildWnd 파생 클래스와 CView 파생 클래스, CDocument 파생 클라스가 하나의 템플릿(Template)으로 구성되며, 이러한 탬플릿이 하나 이상으로 구성되는 것이다. 그래서 여러 템플릿 애플리케이션이라고도 한다.

[ 실습 ]

단일문서 생성 후 클래스 뷰 에서 CMFC1View 에서 변수를 추가합니다

**m_strWindowSize** 를 생성 후
형식은 **CString** 으로 잡아줬습니다

그 후 클래스 마법사에서 WM_SIZE 를 선택후 OnSize 메세지 핸들러 코드편집 해줬습니다.

void CMFC1View::OnSize(UINT nType, int cx, int cy)<br>
{<br>
	CView::OnSize(nType, cx, cy);<br>

	// TODO: 여기에 메시지 처리기 코드를 추가합니다.<br>
	//윈도우 크기가 변경될 때 크기를 나타내는 문자열 생성<br>
	m_strWindowSize.Format(_T("윈도우 크기는 넓이 %d, 높이 %d입니다"), cx, cy);<br>
	//화면 갱신<br>
	Invalidate();<br>
}

윈도우 크기가 변경될 때마다 OnSize() 메세지 핸들러 함수의 인자 cx,cy를 이용하여 윈도우 크기를 나타내는 문자열을 생성하고, Invalidate() 함수를 이용하여 OnDraw() 함수를 호출했습니다.

그 후 OnDraw(CDC* pDC) 함수에 윈도우 크기를 나타내는 문자열을 출력하는 코드를 추가했습니다. (매개변수에 주석처리된 부분을 해재)

void CMFC1View::OnDraw(CDC* pDC)<br>
{<br>
	CMFC1Doc* pDoc = GetDocument();<br>
	ASSERT_VALID(pDoc);<br>
	if (!pDoc)<br>
		return;<br>

	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.<br>
	// 윈도우 크기를 나타내는 문자열을 윈도우 촤측 상단(10,10)에 출력<br>
	pDC->TextOut(10, 10, m_strWindowSize);<br>
}
