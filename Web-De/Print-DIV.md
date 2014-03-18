## Javascript 代码

    functin myPrint(obj){
     var newWindow = window.open("打印窗口","_blank");
     var docStr = obj.innerHTML;
     newWindow.document.write(docStr);
     newWindow.document.close();
     newWindow.document.print();
     newWindow.document.close();
    

}

## HTML代码

    <div id="print">
     此处为打印区域
    </div>