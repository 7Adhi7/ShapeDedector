# ShapeDedector
recognises a given shape



import cv2 as cv

strShape = input("Enter a shape : ")
string=strShape.upper()

def getContours(img):
    contours,hierarchy = cv.findContours(img,cv.RETR_EXTERNAL,cv.CHAIN_APPROX_NONE)
    
    for cnt in contours:
        area = cv.contourArea(cnt)
        if area>500:
            perimeter = cv.arcLength(cnt,True)
            approx = cv.approxPolyDP(cnt,0.01*perimeter,True)
            objCor = len(approx)
            x, y, w, h = cv.boundingRect(approx)

            if objCor ==3: objectType ="Triangle"
            elif objCor == 4:
                aspRatio = w/float(h)
                if aspRatio >0.80 and aspRatio <1.20: objectType= "Square"
                else:objectType="Rectangle"
            
            elif objCor==5: objectType= "Pentagon"
            elif objCor==6: objectType= "Hexagon"
            elif objCor==7: objectType= "Heptagon"
            elif objCor==8: objectType= "Octagon"
            else:
               aspRatio = w/float(h)
               if aspRatio >0.95 and aspRatio <1.05: objectType= "circle"
               else:objectType="Ellipse"

            s=objectType.upper()
            if s==string: 
                print(objectType)
                cv.rectangle(imgContour,(x,y),(x+w,y+h),(0,0,255),5)
                cv.imshow('out',imgContour)
                cv.waitKey(10000)
            
  
img = cv.imread('shapes.jpg')
imgContour = img.copy()
imgG = cv.cvtColor(img,cv.COLOR_BGR2GRAY)
imgC = cv.Canny(imgG,50,50)
getContours(imgC)
 
