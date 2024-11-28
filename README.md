# 4주차 Study [ 2024-11-25 ~ 2024-11-29 ]

**[ 목표 ]**
- 이번주 목표 : 단일 문서 기반 프로그램 제작작
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
	CView::OnSize(nType, cx, cy);<b

	// TODO: 여기에 메시지 처리기 코드를 추가합니다.
	//윈도우 크기가 변경될 때 크기를 나타내는 문자열 생성
	m_strWindowSize.Format(_T("윈도우 크기는 넓이 %d, 높이 %d입니다"), cx, cy);
	//화면 갱신
	Invalidate();
}

윈도우 크기가 변경될 때마다 OnSize() 메세지 핸들러 함수의 인자 cx,cy를 이용하여 윈도우 크기를 나타내는 문자열을 생성하고, Invalidate() 함수를 이용하여 OnDraw() 함수를 호출했습니다.

그 후 OnDraw(CDC* pDC) 함수에 윈도우 크기를 나타내는 문자열을 출력하는 코드를 추가했습니다. (매개변수에 주석처리된 부분을 해재)

void CMFC1View::OnDraw(CDC* pDC)<br>
{<br>
	CMFC1Doc* pDoc = GetDocument();<br>
	ASSERT_VALID(pDoc);<br>
	if (!pDoc)<br>
		return;<br>

	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.
	// 윈도우 크기를 나타내는 문자열을 윈도우 촤측 상단(10,10)에 출력
	pDC->TextOut(10, 10, m_strWindowSize);
}

결과 : https://github.com/Hancho0/4-study/blob/main/MFC(1).png

[ 실습 2 ]

키보드 마우스 동작에 따라 출력

m_strOutput 변수에 CString 형식으로 생성 후

OnLButtonDown() 메시지 핸들러에다 을 추가하여

void CMFC1View::OnLButtonDown(UINT nFlags, CPoint point)<br>
{<br>
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.<br>
	m_strOutput = _T("왼쪽 마우스 버튼을 눌렀습니다.");<br>
	Invalidate();<br>

	CView::OnLButtonDown(nFlags, point);
}

코드를 작성해줍니다.

OnRButtonDown() 메시지 핸들러에다 을 추가하여

void CMFC1View::OnRButtonDown(UINT nFlags, CPoint point)<br>
{<br>
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.<br>
	m_strOutput = _T("오른쪽 마우스 버튼을 눌렀습니다.");<br>
	Invalidate();<br>

	CView::OnRButtonDown(nFlags, point);
}

코드를 작성해줍니다.

그리고 OnKeyDown() 메시지 핸들러 함수를 선택한 후

void CMFC1View::OnKeyDown(UINT nChar, UINT nRepCnt, UINT nFlags)
{
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.
	m_strOutput = _T("키보드를 눌렀습니다.");
	Invalidate();

	CView::OnKeyDown(nChar, nRepCnt, nFlags);
}

코드를 작성해줍니다.

그리고 난 후 m_bDrag 변수를 bool 형식으로 하나 추가해준 후

OnLButtonDown() 메시지 핸들러에다

void CMFC1View::OnLButtonDown(UINT nFlags, CPoint point)<br>
{<br>
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.<br>
 	**m_bDrag = TRUE;**<br>
	m_strOutput = _T("왼쪽 마우스 버튼을 눌렀습니다.");<br>
	Invalidate();<br>

	CView::OnLButtonDown(nFlags, point);
}

코드를 추가 작성해줍니다.

그리고 난 후 OnMouseMove 메시지 핸들러 함수를 추가한 후

void CMFC1View::OnMouseMove(UINT nFlags, CPoint point)<br>
{
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.<br>
	if (m_bDrag == TRUE) { //마우스가 클릭 된 상태일 때<br>
		m_strOutput = _T("마우스를 드래그하고 있습니다.");<br>
	}<br>
	else { //마우스가 클릭 되지 않은 상태일 때<br>
		m_strOutput = _T("마우스를 이동 중입니다.");<br>
	}<br>
	Invalidate();<br>

	CView::OnMouseMove(nFlags, point);
}

라는 코드를 작성해줍니다.

그리고 나서 OnLButtonUp() 메시지 핸들러 함수에다가

void CMFC1View::OnRButtonUp(UINT nFlags, CPoint point)<br>
{<br>
	// TODO: 여기에 메시지 처리기 코드를 추가 및/또는 기본값을 호출합니다.<br>
	m_bDrag = FALSE;<br>

	CView::OnRButtonUp(nFlags, point);
}

를 넣어 마우스 버튼을 글릭하고 있지 않을땐 드래그를 FALSE로 인식하지 않게 합니다.

마지막으로 OnDRaw(CDC* pDC) 함수에

void CMFC1View::OnDraw(CDC* pDC)<br>
{<br>
	CMFC1Doc* pDoc = GetDocument();<br>
	ASSERT_VALID(pDoc);<br>
	if (!pDoc)<br>
		return;<br>

	// TODO: 여기에 원시 데이터에 대한 그리기 코드를 추가합니다.
	// 윈도우 크기를 나타내는 문자열을 윈도우 촤측 상단(10,10)에 출력
	pDC->TextOut(10, 10, m_strWindowSize);
	CRect rect;
	GetClientRect(&rect);
	pDC->DrawText(m_strOutput, rect, DT_SINGLELINE | DT_CENTER | DT_VCENTER);
}

코드를 추가 작성 해줍니다.

CRect 클래스 : windows RECT 구조와 유사하며 사각형의 좌측 상단과 우측하단의 좌표를 저장하기 위한 클래스<br>
GetClientRect() 함수 : 윈도우의 클라이언트 영역의 크기를 얻는 함수<br>
DT_SINGLELINE : 행바꿈과 라인피드를 무시하고 한 줄로 출력 | DT_CENTER : 설정된 영역의 가로 중앙에 정렬 | DT_VCENTER : 설정된 영역의 세로 중앙에 정렬(DT_SINGLELINE과 함께 지정되어야 한다.)

그리하면 화면 상단엔 윈도우의 크기가 출력되며, 키보드를 누를 경우 중앙에 "키보드를 눌렀습니다." 뜨고 마우스 왼쪽,오른쪽 눌렀을 경우 중앙에 "왼쪽(오른쪽)마우스 버튼을 눌렀습니다." 뜨고,
마우스가 이동하는 경우에 "마우스를 이동중 입니다." 뜨며 클릭하면서 드래그 하면 "마우스를 드래그하고 있습니다." 가 뜹니다.

-----
**[ 이번 Study를 통해 느낀점 ]**
이번주는 단일 문서 기반 프로그램 제작을 해보았습니다. 부족한 부분과 아직 모르는 부분에 대해서 좀 더 공부를 해볼 생각입니다. 그리고 다음주에는 메세지 처리에 대해 해볼려고 합니다. 생각보다 단일 문서 기반 프로그램 제작에 여러움을 느끼진 않았으며 생각보다 재밌게 배웠던거 같습니다.
