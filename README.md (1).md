# Word editor report
In this report we will implement the knowledge we got from working on the spreadsheet 
## Wordtext.cpp
```cpp
#include  "wordtext.h"

#include  "ui_wordtext.h"

#include  <QTextStream>

  

wordtext::wordtext(QWidget  *parent)

  :  QMainWindow(parent)

  ,  ui(new  Ui::wordtext)

{

  ui->setupUi(this);

}

  

wordtext::~wordtext()

{

  delete  ui;

}

  

  

  

  

  

void  wordtext::on_actionOpen_triggered()

{

  QFileDialog  l  ;

  currentFile=  l.getOpenFileName();

  

  setWindowTitle(currentFile);

  

  loadContent(currentFile);

  

  

  

  

}

  

void  wordtext::saveContent(QString  filename)const

{

  

  QFile  file(filename);

  

  

  if(file.open(QIODevice::WriteOnly))

  

  {  auto  text  =  ui->plainTextEdit->toPlainText();

  

  QTextStream  in(&file);

  

  

  in  <<  text  ;

  }

  

  

  file.close();

  

}

void  wordtext::loadContent(QString  filename){

  QFile  file(filename);

  if  (file.open(QIODevice::ReadOnly  ))  {

  QTextStream  in(&file);

  

  

  

  

  while  (!in.atEnd())  {

  QString  line  =  in.readLine();

  ui->plainTextEdit->textCursor().insertText(line);

ui->plainTextEdit->textCursor().insertText("\n");

  

  

  }

}

  

}

  

  

  

  

  

  

  

void  wordtext::on_actionSave_triggered()

{

  

  auto  dialog  =  new  QFileDialog(this);

  

  

  if(currentFile  ==  "")

  {

  currentFile  =  dialog->getSaveFileName(this,"choose your file");

  

  

  setWindowTitle(currentFile);

  

  }

  

  

  if(  currentFile  !=  "")

  {

  saveContent(currentFile);

  }

  

}
```
## Wordtext.h
```cpp
#ifndef  WORDTEXT_H

#define  WORDTEXT_H

  

#include  <QMainWindow>

#include  <QFileDialog>

  

  

QT_BEGIN_NAMESPACE

namespace  Ui  {  class  wordtext;  }

QT_END_NAMESPACE

  

class  wordtext  :  public  QMainWindow

{

  Q_OBJECT

  

public:

  wordtext(QWidget  *parent  =  nullptr);

  ~wordtext();

  

private  slots:

  

  void  on_actionOpen_triggered();

  

  void  on_actionSave_triggered();

  

private:

  Ui::wordtext  *ui;

  QString  currentFile=NULL;

  void  loadContent(QString  currentFile);

  void  saveContent(QString  currentFile)  const;

  

};

#endif  //  WORDTEXT_H
 
```
## Conclusion
We did set the menu bar the same as the previous project with it s various tools as well 

