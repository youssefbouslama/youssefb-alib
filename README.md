# youssefb-alib
    # Spreadsheet Report

### Overview :



* Creating Spreadsheet
* Search engine
* Find Cell
> as long as saving , selecting all and other tools as well .

## Speadsheet.h

```cpp
#ifndef  SPREADSHEET_H

#define  SPREADSHEET_H

  

#include  <QMainWindow>

#include  <QTableWidget>

#include  <QAction>

#include  <QMenu>

#include  <QToolBar>

#include  <QLabel>

#include  <QStatusBar>

#include  "gocell.h"

#include  "finddialog.h"

  

class  SpreadSheet  :  public  QMainWindow

{

  Q_OBJECT

  

public:

  SpreadSheet(QWidget  *parent  =  nullptr);

  ~SpreadSheet();

  

  

private:

  void  savecontent(QString  filename);  //methode  pour  sauvegarder

  void  loadcontent(QString  filename);

  

protected:

  void  setupMainWidget();

  void  createActions();

  void  createMenus();

  void  createToolBars();

  void  makeConnexions();

  

private  slots:

  void  close();

  void  updateStatusBar(int,  int);  //Respond  for  the  call  changed

  void  goCellSlot();

  void  findSlot();

  void  saveSlot();

  void  load();

  

  //Pointers

private:

  //  ---------------  Central  Widget  -------------//

  QTableWidget  *spreadsheet;

  //  ---------------  Actions  --------------//

  QAction  *  newFile;

  QAction  *  open;

  QAction  *  save;

  QAction  *  saveAs;

  QAction  *  exit;

  QAction  *cut;

  QAction  *copy;

  QAction  *paste;

  QAction  *deleteAction;

  QAction  *find;

  QAction  *row;

  QAction  *Column;

  QAction  *all;

  QAction  *goCell;

  QAction*findDialog;

  QAction  *recalculate;

  QAction  *sort;

  QAction  *showGrid;

  QAction  *auto_recalculate;

  QAction  *about;

  QAction  *aboutQt;

  

  

  //  ----------  Menus  ----------

  QMenu  *FileMenu;

  QMenu  *editMenu;

  QMenu  *toolsMenu;

  QMenu  *optionsMenu;

  QMenu  *helpMenu;

  

  

  //  -----  -  Widget  pouyr  la  bare  d'etat

  QLabel  *cellLocation;  //position  de  la  cellule  active

  QLabel  *cellFormula;  //  Formuel  de  la  cellue  active

  

  //-----  nom  du  fichier  courant

  QString  *currentFile;

  

  

};

  

#endif  //  SPREADSHEET_H
```

## Spreadsheet.cpp
```cpp
#include  "spreadsheet.h"

#include  <QPixmap>

#include  <QMenuBar>

#include  <QToolBar>

#include  <QApplication>

#include  <QMessageBox>

#include  <QFileDialog>

#include  "finddialog.h"

#include  "gocell.h"

#include  <QTextStream>

  

  

  

SpreadSheet::SpreadSheet(QWidget  *parent)

  :  QMainWindow(parent)

{

  //Seting  the  spreadsheet

  

  setupMainWidget();

  

  //  Creaeting  Actions

  createActions();

  

  //  Creating  Menus

  createMenus();

  

  

  //Creating  the  tool  bar

  createToolBars();

  

  //making  the  connexions

  makeConnexions();

  

  

  //Creating  the  labels  for  the  status  bar  (should  be  in  its  proper  function)

  cellLocation  =  new  QLabel("(1, 1)");

  cellFormula  =  new  QLabel("");

  statusBar()->addPermanentWidget(cellLocation);

  statusBar()->addPermanentWidget(cellFormula);

  

  //  initier  le  nom  du  fichier

  currentFile  =  nullptr;

  //mettre  le  nom  du  spreadsheet

  setWindowTitle("Buffer");

}

  

void  SpreadSheet::setupMainWidget()

{

  spreadsheet  =  new  QTableWidget;

  spreadsheet->setRowCount(100);

  spreadsheet->setColumnCount(10);

  setCentralWidget(spreadsheet);

  

}

  

SpreadSheet::~SpreadSheet()

{

  delete  spreadsheet;

  

  //  ---------------  Actions  --------------//

  delete  newFile;

  delete  open;

  delete  save;

  delete  saveAs;

  delete  exit;

  delete  cut;

  delete  copy;

  delete  paste;

  delete  deleteAction;

  delete  find;

  delete  row;

  delete  Column;

  delete  all;

  delete  goCell;

  delete  recalculate;

  delete  sort;

  delete  showGrid;

  delete  auto_recalculate;

  delete  about;

  delete  aboutQt;

  

  //  ----------  Menus  ----------

  delete  FileMenu;

  delete  editMenu;

  delete  toolsMenu;

  delete  optionsMenu;

  delete  helpMenu;

}

  

void  SpreadSheet::createActions()

{

  //  ---------  New  File  -------------------

  QPixmap  newIcon(":/new_file.png");

  newFile  =  new  QAction(newIcon,  "&New",  this);

  newFile->setShortcut(tr("Ctrl+N"));

  

  

  //  ---------  open  file  -------------------

  open  =  new  QAction("&Open",  this);

  open->setShortcut(tr("Ctrl+O"));

  

  //  ---------  open  file  -------------------

  save  =  new  QAction("&Save",  this);

  save->setShortcut(tr("Ctrl+S"));

  

  //  ---------  open  file  -------------------

  saveAs  =  new  QAction("save &As",  this);

  

  

  //  ---------  open  file  -------------------

  QPixmap  cutIcon(":/cut_icon.png");

  cut  =  new  QAction(newIcon,  "Cu&t",  this);

  cut->setShortcut(tr("Ctrl+X"));

  

  //  ---------  Cut  menu  -----------------

  copy  =  new  QAction(  "&Copy",  this);

  copy->setShortcut(tr("Ctrl+C"));

  

  

  paste  =  new  QAction(  "&Paste",  this);

  paste->setShortcut(tr("Ctrl+V"));

  

  

  deleteAction  =  new  QAction(  "&Delete",  this);

  deleteAction->setShortcut(tr("Del"));

  

  

  row  =  new  QAction("&Row",  this);

  Column  =  new  QAction("&Column",  this);

  all  =  new  QAction("&All",  this);

  all->setShortcut(tr("Ctrl+A"));

  

  

  

  QPixmap  findIcon(":/search_icon.png");  find=  new  QAction(newIcon,  "&Find",  this);

  find->setShortcut(tr("Ctrl+F"));

  

  QPixmap  goCellIcon(":/go_to_icon.png");

  goCell  =  new  QAction(  goCellIcon,  "&Go to Cell",  this);

  deleteAction->setShortcut(tr("f5"));

  

  

  recalculate  =  new  QAction("&Recalculate",this);

  recalculate->setShortcut(tr("F9"));

  

  

  sort  =  new  QAction("&Sort");

  

  

  

  showGrid  =  new  QAction("&Show Grid");

  showGrid->setCheckable(true);

  showGrid->setChecked(spreadsheet->showGrid());

  

  auto_recalculate  =  new  QAction("&Auto-recalculate");

  auto_recalculate->setCheckable(true);

  auto_recalculate->setChecked(true);

  

  

  

  about  =  new  QAction("&About");

  aboutQt  =  new  QAction("About &Qt");

  

  //  ---------  exit  -------------------

  QPixmap  exitIcon(":/quit_icon.png");

  exit  =  new  QAction(exitIcon,"E&xit",  this);

  exit->setShortcut(tr("Ctrl+Q"));

}

  

void  SpreadSheet::close()

{

  

  auto  reply  =  QMessageBox::question(this,  "Exit",

  "Do you really want to quit?");

  if(reply  ==  QMessageBox::Yes)

  qApp->exit();

}

  

void  SpreadSheet::createMenus()

{

  //  --------  File  menu  -------//

  FileMenu  =  menuBar()->addMenu("&File");

  FileMenu->addAction(newFile);

  FileMenu->addAction(open);

  FileMenu->addAction(save);

  FileMenu->addAction(saveAs);

  FileMenu->addSeparator();

  FileMenu->addAction(exit);

  

  

  //-------------  Edit  menu  --------/

  editMenu  =  menuBar()->addMenu("&Edit");

  editMenu->addAction(cut);

  editMenu->addAction(copy);

  editMenu->addAction(paste);

  editMenu->addAction(deleteAction);

  editMenu->addSeparator();

  auto  select  =  editMenu->addMenu("&Select");

  select->addAction(row);

  select->addAction(Column);

  select->addAction(all);

  

  editMenu->addAction(find);

  editMenu->addAction(goCell);

  

  

  

  //--------------  Toosl  menu  ------------

  toolsMenu  =  menuBar()->addMenu("&Tools");

  toolsMenu->addAction(recalculate);

  toolsMenu->addAction(sort);

  

  

  

  //Optins  menus

  optionsMenu  =  menuBar()->addMenu("&Options");

  optionsMenu->addAction(showGrid);

  optionsMenu->addAction(auto_recalculate);

  

  

  //-----------  Help  menu  ------------

  helpMenu  =  menuBar()->addMenu("&Help");

  helpMenu->addAction(about);

  helpMenu->addAction(aboutQt);

}

  

void  SpreadSheet::createToolBars()

{

  

  //Crer  une  bare  d'outils

  auto  toolbar1  =  addToolBar("File");

  

  

  //Ajouter  des  actions  acette  bar

  toolbar1->addAction(newFile);

  toolbar1->addAction(save);

  toolbar1->addSeparator();

  toolbar1->addAction(exit);

  

  

  //Creer  une  autre  tool  bar

  auto  toolbar2  =  addToolBar("ToolS");

  toolbar2->addAction(goCell);

  auto  toolbar3  =  addToolBar("ToolS");

  toolbar3->addAction(find);

}

  

void  SpreadSheet::updateStatusBar(int  row,  int  col)

{

  QString  cell{"(%0, %1)"};

  cellLocation->setText(cell.arg(row+1).arg(col+1));

  

}

  

void  SpreadSheet::makeConnexions()

{

  

  //Connexion  for  the  select  all  action

  connect(all,  &QAction::triggered,

  spreadsheet,  &QTableWidget::selectAll);

  

  //  Connection  for  the  show  grid

  connect(showGrid,  &QAction::triggered,

  spreadsheet,  &QTableWidget::setShowGrid);

  

  //Connection  for  the  exit  button

  connect(exit,  &QAction::triggered,  this,  &SpreadSheet::close);

  

  

  //connectting  the  chane  of  any  element  in  the  spreadsheet  with  the  update  status  bar

  connect(spreadsheet,  &QTableWidget::cellClicked,  this,  &SpreadSheet::updateStatusBar);

  

  //connexion  between  gotocell  action  and  gotocell  slot

  connect(goCell,  &QAction::triggered,  this,  &SpreadSheet::goCellSlot);

  

  //connexion  between  find  action  and  find  slot

  connect(find,  &QAction::triggered,  this,  &SpreadSheet::findSlot);

  

  //connexion  de  save  file

  connect(save,  &QAction::triggered,  this,  &SpreadSheet::saveSlot);

  

  connect(open,  &QAction::triggered,  this,  &SpreadSheet::load);

}

  

void  SpreadSheet::goCellSlot()

{

  //Creating  the  dialog

  GoCell  D;

  

  //Executing  the  dialog  and  storing  the  user  response

  auto  reply  =  D.exec();

  

  //Checking  if  the  dialog  is  accepted

  if(reply  ==  GoCell::Accepted)

  {

  

  //Getting  the  cell  text

  auto  cell  =  D.cell();

  

  //letter  distance

  int  row  =  cell[0].toLatin1()  -  'A';

  cell.remove(0,1);

  

  //second  coordinate

  int  col  =  cell.toInt();

  

  

  //changing  the  current  cell

  spreadsheet->setCurrentCell(row,  col-1);

  }

}

void  SpreadSheet::findSlot()

{

  

  FindDialog  f;

  auto  reply  =  f.exec();

  if(reply  ==  FindDialog::Accepted)

  {

  

  auto  cellfield  =  f.textfield();

  

  for(int  i=0;i<spreadsheet->rowCount();i++)

  {

  for(int  j=0;j<spreadsheet->columnCount();j++)

  {

  if(spreadsheet->item(i,j))

  {

  if(spreadsheet->item(i,j)->text()==cellfield)

  spreadsheet->setCurrentCell(  i,  j);

  }

  }

  }

  }

}

void  SpreadSheet::saveSlot(){

  //verifier  si  on  a  pas  un  nom  de  fichier

  if(!currentFile)

  {

  QFileDialog  D;

  auto  filename  =  D.getSaveFileName();

  QMessageBox::information(this,  "title",  "you chossed the file"+filename);

  

  currentFile  =  new  QString(filename);

  

  setWindowTitle(*currentFile);

  }

  

  //fonction  privee  pour  sauvegarder  le  contenu

  savecontent(*currentFile);

}

void  SpreadSheet::savecontent(QString  filename)

{

  //pointeur  sur  un  fichier

  QFile  file(filename);

  

  //ouvrir  le  fichier  en  mode  lecture

  if(file.open(QIODevice::WriteOnly))

  {

  QTextStream  out(&file);

  //boucle  sur  les  cellules

  for(int  i=0;  i<  spreadsheet->rowCount();i++)

  for(int  j=0;  j<spreadsheet->columnCount();j++){

  auto  cell  =  spreadsheet  ->  item(i,j);

  if(cell)

  {

  out<<i<<","<<j<<","<<cell->text()<<endl;

  }

  }

  

  

  }

  

  //fermer  la  connexion  avec  le  fichier

  file.close();

}

void  SpreadSheet::load(){

  

QFileDialog  D;

auto  filename  =  D.getOpenFileName();

  

currentFile  =  new  QString(filename);

setWindowTitle(*currentFile);

  

loadcontent(filename);

  

}

void  SpreadSheet::loadcontent(QString  filename)

{

  

  QFile  file  (filename);

  if(file.open(QIODevice::ReadOnly))

  {

  QTextStream  in(&file);

  

  while(!in.atEnd())

  {

  QString  line;

  line  =  in.readLine();

  

  auto  tokens  =  line.split(QChar(','));

  

  int  row  =  tokens[0].toInt();

  int  col  =  tokens[1].toInt();

  auto  cell  =new  QTableWidgetItem(tokens[2]);

  spreadsheet->setItem(row,  col,  cell);

  }

  }

  

}
```


## FindCell.h
```cpp
#ifndef  GOCELL_H

#define  GOCELL_H

  

#include  <QDialog>

  

namespace  Ui  {

class  GoCell;

}

  

class  GoCell  :  public  QDialog

{

  Q_OBJECT

  

public:

  explicit  GoCell(QWidget  *parent  =  nullptr);

  QString  cell()  const;

  

  ~GoCell();

  

private:

  Ui::GoCell  *ui;

  

};

  

#endif  //  GOCELL_H
```

## FindCell.cpp
```cpp
#include  "gocell.h"

#include  "ui_gocell.h"

#include  <QRegExpValidator>

#include  <QRegExp>

  

  

GoCell::GoCell(QWidget  *parent)  :

  QDialog(parent),

  ui(new  Ui::GoCell)

{

  ui->setupUi(this);

  //Validating  the  regular  expression

  QRegExp  regCell{"[A-Z][1-9][0-9]{0,2}"};

  

  //Validating  the  regular  expression

  ui->lineEdit->setValidator(new  QRegExpValidator(regCell));

}

  

GoCell::~GoCell()

{

  delete  ui;

}

QString  GoCell::cell()  const

  {

  return  ui->lineEdit->text();

  }
```


## FindDialog.h
```cpp
#ifndef  FINDDIALOG_H

#define  FINDDIALOG_H

  

#include  <QDialog>

  

namespace  Ui  {

class  FindDialog;

}

  

class  FindDialog  :  public  QDialog

{

  Q_OBJECT

  

public:

  explicit  FindDialog(QWidget  *parent  =  nullptr);

  QString  textfield()  const;

  ~FindDialog();

  

private:

  Ui::FindDialog  *ui;

};

  

#endif  //  FINDDIALOG_H
```
## FindDialog.cpp
```cpp
#include  "finddialog.h"

#include  "ui_finddialog.h"

  

FindDialog::FindDialog(QWidget  *parent)  :

  QDialog(parent),

  ui(new  Ui::FindDialog)

{

  ui->setupUi(this);

}

  

FindDialog::~FindDialog()

{

  delete  ui;

}

QString  FindDialog::textfield()  const

{

  return  ui->lineEdit->text();

}
```

# Conclusion 

This course was very practical , we got to dive in a lot of stuff related to **UI Widgets** , we approached it through the interface as long as the manual way **''Coding''** which was a bit harder , but overall it was a good experience.

    
