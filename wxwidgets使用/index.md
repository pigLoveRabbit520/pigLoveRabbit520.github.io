# wxWidgets使用

![](https://www.wxwidgets.org/assets/img/header-logo.png)

<!-- more -->

## 源码编译安装
前往wxWidgets[官网](https://www.wxwidgets.org/downloads/)，下载wxWidgets

![upload successful](/images/wxWidgets_home.png)

打开`Developer Command Prompt for VS 2022`工具，进入 `build\msw` 目录，编译
```
 nmake /f makefile.vc BUILD=debug SHARED=0 TARGET_CPU=X64
```
这里我编译了64位debug版的静态库，这里`x64`和`debug`两个关键字很重要，你在VS中开发时，也要选择相应的配置  
![tip](https://user-images.githubusercontent.com/16663435/175751439-7e86aa58-4ba4-4b8f-8082-25fb3a7d0070.png "tip")  

在**属性管理器**窗口添加**wxWidgets**目录下的wxwidgets.props文件  
![tip](https://user-images.githubusercontent.com/16663435/175751577-a88e9bbb-c3a2-4321-a93b-3c2ae26be0b6.png "tip")  
这里我们还需要额外注意一下，因为我们跑的是GUI程序，所以需要在项目的**属性**中，把项目设置成`窗口`
![upload](/images/wxWidgets_vs_settings.jpg)  
跑个简单的程序，hello.cpp
```
// wxWidgets "Hello world" Program
// For compilers that support precompilation, includes "wx/wx.h".
#include <wx/wxprec.h>
#ifndef WX_PRECOMP
#include <wx/wx.h>
#endif
class MyApp : public wxApp
{
public:
    virtual bool OnInit();
};
class MyFrame : public wxFrame
{
public:
    MyFrame(const wxString& title, const wxPoint& pos, const wxSize& size);
private:
    void OnHello(wxCommandEvent& event);
    void OnExit(wxCommandEvent& event);
    void OnAbout(wxCommandEvent& event);
    wxDECLARE_EVENT_TABLE();
};
enum
{
    ID_Hello = 1
};
wxBEGIN_EVENT_TABLE(MyFrame, wxFrame)
EVT_MENU(ID_Hello, MyFrame::OnHello)
EVT_MENU(wxID_EXIT, MyFrame::OnExit)
EVT_MENU(wxID_ABOUT, MyFrame::OnAbout)
wxEND_EVENT_TABLE()
wxIMPLEMENT_APP(MyApp);
bool MyApp::OnInit()
{
    MyFrame* frame = new MyFrame("Hello World", wxPoint(50, 50), wxSize(450, 340));
    frame->Show(true);
    return true;
}
MyFrame::MyFrame(const wxString& title, const wxPoint& pos, const wxSize& size)
    : wxFrame(NULL, wxID_ANY, title, pos, size)
{
    wxMenu* menuFile = new wxMenu;
    menuFile->Append(ID_Hello, "&Hello...\tCtrl-H",
        "Help string shown in status bar for this menu item");
    menuFile->AppendSeparator();
    menuFile->Append(wxID_EXIT);
    wxMenu* menuHelp = new wxMenu;
    menuHelp->Append(wxID_ABOUT);
    wxMenuBar* menuBar = new wxMenuBar;
    menuBar->Append(menuFile, "&File");
    menuBar->Append(menuHelp, "&Help");
    SetMenuBar(menuBar);
    CreateStatusBar();
    SetStatusText("Welcome to wxWidgets!");
}
void MyFrame::OnExit(wxCommandEvent& event)
{
    Close(true);
}
void MyFrame::OnAbout(wxCommandEvent& event)
{
    wxMessageBox("This is a wxWidgets' Hello world sample",
        "About Hello World", wxOK | wxICON_INFORMATION);
}
void MyFrame::OnHello(wxCommandEvent& event)
{
    wxLogMessage("Hello world from wxWidgets!");
}
```
