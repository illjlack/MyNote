#### qt中文

```cpp
// =====================================================中文
#include <QTextCodec>

inline QString L(const char* str) 
{
    static QTextCodec* codec = QTextCodec::codecForName("GBK");
    return codec->toUnicode(str);
}


// 或者
#define L(str) QString::fromUtf8(str)
```

#### qt简单日志

```cpp
// =====================================================日志
#include <QDebug>
#include <QString>
#include <sstream>
#include <filesystem>

class LogHelper
{
public:
    inline LogHelper(const char* file, int line)
        : file_(file), line_(line) {}

    template <typename T>
    LogHelper& operator<<(const T& value)
    {
        stream_ << value;
        return *this;
    }

    inline LogHelper& operator<<(const QString& value) 
    {
        stream_ << value.toStdString();
        return *this;
    }

    inline ~LogHelper()
    {
        qDebug() << "Log:" << QString::fromStdString(stream_.str())
            << ", file:" << QString::fromStdString(std::filesystem::path(file_).filename().string()) << ", line:" << line_;
    }

private:
    std::ostringstream stream_;
    const char* file_;
    int line_;
};

// 流式宏定义
#define Log LogHelper(__FILE__, __LINE__)
```

#### qt计时器

```cpp
#include <QElapsedTimer>

QElapsedTimer timer;
timer.start();

qDebug() << QString("[...] ...: %1 ms").arg(timer.restart());
// 返回与上次开始的时间差
qDebug() << QString("[...] ...: %1 ms").arg(timer.restart()));

// 总用时
qDebug() << QString("[...] Timing: %1 s.").arg(timer.elapsed() / 1000.0, 0, 'f', 1)
```

#### qt进度条

```cpp
    QProgressDialog progressDialog(parent);
    progressDialog.setWindowTitle("...");
    progressDialog.setLabelText("...");
    progressDialog.setRange(0, maxIterations);  // 设置进度条的范围
    progressDialog.show();
    QCoreApplication::processEvents();  // 允许UI更新

    bool wasCancelled = false;

    for (int i = 0; i < maxIterations; ++i)
    {
        progressDialog.setValue(i);
        QCoreApplication::processEvents();  // 允许UI更新

        if (progressDialog.wasCanceled())
        {
            wasCancelled = true;
            break;
        }
    }

    progressDialog.close();  // 关闭进度条
    QCoreApplication::processEvents();  // 允许UI更新
```



