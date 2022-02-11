MotionEvent中getAction()与getActionMasked()的区别
    如果mAction的值是在0x00到0xff之间, getAction()和getActionMasked()的返回的值是一样的.
    当有多点触控时mAction的值会大于0xff, mAction的低8位(也就是0-7位)是动作类型信息, mAction的高8位(也就是8-15位)是触控点的索引信息(即表示是哪一个触控点的事件).
    public static final int ACTION_MASK                = 0xff;
    public static final int ACTION_POINTER_ID_MASK     = 0x00ff;//低8位, (0-7位)是动作类型信息
    public static final int ACTION_POINTER_INDEX_MASK  = 0xff00;//高8位, (8-15位)是触控点的索引信息(即表示第几个触控点)
    例如:
        mAction=0x0000, 表示第一个触控点的ACTION_DOWN操作.
        mAction=0x0100, 表示第二个触控点的ACTION_DOWN操作.
        mAction=0x0200, 表示第三个触控点的ACTION_DOWN操作.
    为什么不用两个字段来表示? 如用int mAction表示动作类型, 用int mPointer表示第几个触控点?
    注:
        因为动作类型只需要0-255就能表示全部, 触控点信息mPointer也只需要0-255就能全部表示, 用一个字段只占32位, 用两个字段需要占32*2=64位, 即节约内存,又方便提高处理速度.
        通常我们是以不同的字段来存储不同的信息, 但是计算机内部始终是以位来存储信息, 这对于理解android中的很多变量是很有帮助的, 因为有很多变量使用这种技巧来节约内存.
        onMeasure中的MeasureSpec也是这么存储的.

MotionEvent中getX()与getRawX()的区别
    MotionEvent.getX():获取的是点击事件距离控件自身左边的距离
    MotionEvent.getY():获取的是点击事件距离控件自身上边的距离
    MotionEvent.getRawX():获取的是触摸点距离屏幕左边界的距离
    MotionEvent.getRawY():获取的是触摸点距离屏幕上边界的距离 
    View.getWidth():获取的是当前控件的宽度, getRight()-getLeft()
    View.getHeight()：获取的是当前控件的高度, getBottom()-getTop() 
    View.getLeft(): 获取的是View自身的左边到父View左边的距离
    View.getTop(): 获取的是View自身的顶部到父View顶部的距离
    View.getRight(): 获取的是View自身的右边到父View左边的距离
    View.getBottom(): 获取的是View自身的底部到父View顶部的距离
    View.getTranslationX()获取的是该View在X轴的偏移量(初始值为0，向左偏移值为负，向右偏移值为正)
    View.getTranslationY()获取的是该View在Y轴的偏移量(初始值为0，向上偏移为负，向下偏移为证)

