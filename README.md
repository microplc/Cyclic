# Cyclic

很小的毫秒基础循环控制的 Arduino 库，很容易使用，包括4个例子。

**仅在 Arduino Due 上测试过**

## 介绍

引入头文件到你的程序

    #include <Cyclic.h>

### 构造

#### 手动循环对象，当你想重启循环的时候需要复位它。

    Cyclic objectName = Cyclic();


#### 自动复位循环对象，每次达到最大时间会自动复位，当然你也可以在任何时候手动复位它。

    Cyclic objectName(maximumTime); // In milliseconds.


#### 有限自动循环，到达 maximumTime 自动复位，手动复位只能在 minimumTime - maximumTime 之间进行。

    Cyclic objectName(maximumTime, minimumTime); // In milliseconds.

### 函数9个，其中有的函数有些相似，所有函数都很简单并易于理解

#### start()

启动循环，一些项目中你想你想尽快立即启动循环，Arduino 可以放入 setup 程序段，另一些时候你希望某些函数运行时启动循环，本函数无参数，需要时调用即可。

    objectName.start();

#### stop()

停止循环，复位所有运行时变量，重启完成次数变量**cycles()**。

    objectName.stop*();

#### status()

运行时返回True，否则返回False，允许在循环启动后或未启动时使用，一般是作为条件判断使用。

    if (objectName.status) {
      objectName.stop;
    }

#### update()

本函数会调用 millis() 来更新循环时钟，由于每次运行它都会调用 millis()，因此仅仅在主 Loop 中执行它或者调用 Cyclic.time() 时执行它。

    objectName.update();

#### reset()

重置循环，循环就像一个精密仪器，从0计数到 maximumTime 后自动重置。

    objectName.reset();

#### reboot()

清除任何变量，重启循环。

    objectName.reboot();

#### time()

返回存储的最后一次调用 update() 之后的时间。

    objectName.update(); // Gets the current time from millis();
    objectName.time(); // Returns the last update() stored time, ex: 300 ms
    objectName.time(); // Returns exactly the same stored time, ex: 300 ms
    objectName.update(); // Gets the current time from millis();
    objectName.time(); // Returns the time got on the last update(), ex: 350 ms

#### now()

实际与时间函数相似，是为懒人准备的， now() 总是调用 update() 在返回时间之前，因此可以单用 now() 而忽略 update() 函数。

    objectName.now(); // Returns the current time from millis(), ex 300 ms
    objectName.now(); // Returns the current time from millis(), ex 301 ms
    objectName.now(); // Returns the current time from millis(), ex 302 ms

#### last()

返回最后一次循环的时间段，如果2s自动复位，last() 返回 2000 ms，如果你手动复位在 1.2 时， last() 返回 1200 ms。

    objectName.last();

#### cycle()

返回你创建循环对象时的最大循环时间。

    objectName.cycle();

#### cycles()

返回循环 start() - stop() 之间的循环次数。记住每次你调用 stop() 会重置这个计数器，如果你需要记录这个次数就需要在 stop() 之前进行。

    objectName.cycles(); // Cycles completed, ex: 30
    totalCycles += objectName.cycles();
    objectName.stop(); // Resets the cycle counter
    objectName.cycles(); // Cycles completed, ex: 0
