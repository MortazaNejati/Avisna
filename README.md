اویسنا(Avisna )  برگرفته از نام دانشمند ایرانی ابوعلی سیناست. این نام برای زبان تخصصی رباتیک از آن جهت برگزیده شد تا تا خرد و روشنایی و علم را همراه با نامی شایسته نشان دهد.
کامپایلر زبان اویسنا از صفر نوشته نشده است و ما اینچنین ادعایی نداریم، این زبان بر روی شانه‌های زبان راست ایستاده است، و می‌کوشد تا خود را بالا بکشد. 
پس این زبان همچون راست زبانی سطح پایین (یا متوسط ) است، که خود را با سخت افزار رودررو می‌بیند.  گرچه سعی شده که  دستور زبان آن تا حد امکان ساده وهمچون زبان انسانی باشد.
fn main() -> int {
return ۵۶؛
}
چرا زبان و کتابخانه نه؟
مهمترین دلیل این جدایی حافظه است. زبان راست بر حافظه برروو چکر  تکیه دارد که خود شاهکاری در معماری حافظه است، اما زبان اویسنا بر حافظه آرنا شیرد  تکیه دارد.  این حافظه همانطور که از نامش پیداست بر منطقه بندی حافظه تکیه دارد. 
دومین دلیل این است که برای آموختن زبان اویسنا(Avisna ) ا

احتیاجی به آموختن زبان راست ندارید، می توانید مستقیم و بدون هیچ داننش قبلی از برنامه نویسی زبان اویسنا را بیاموزید. سعی کرده‌ایم شیب آموزشی و یادگیری آن پایینتر از متوسط باشد.
در حال کار بر روی نسخه ۰۶ هستیم. تا به حال ۲۱۹ چراغ سبز روشن کد .avs   و ۶۷ آزمایش سبز داخلی نتیجه حدود ۴ سال تحقیق، کدنویسی و آزمایش ماست. 
اویسنا چه زمانی برای همه منتشر می شود؟  
اگر نتایج همچنان سبز بمانند با آماده شدن نسخه ۰۷ اولین انتشار آن را اعلام خواهیم کرد. 


The name "Avisna" is derived from that of the Iranian scholar Avicenna (Ibn Sina). This name was chosen for the specialized robotics language to symbolize wisdom, enlightenment, and knowledge through a fitting title.
The Avisna compiler was not written from scratch—we make no such claim; rather, the language stands on the shoulders of Rust, striving to elevate itself further.
Like Rust, Avisna is a low-level (or mid-level) language that interfaces directly with hardware, though we have endeavored to keep its syntax as simple and human-readable as possible.
fn main() -> int {
return 56;
}
Why a separate language rather than just a library?
The primary reason is memory management. Rust relies on the "borrow checker"—a masterpiece of memory architecture—whereas Avisna utilizes "arena-shared memory." As the name implies, this approach relies on memory zoning.
A second reason is that learning Avisna does not require prior knowledge of Rust; you can learn Avisna directly without any previous programming experience. We have aimed for a gentle learning curve.
We are currently working on version 0.6. To date, 219 successful `.avs` code tests and 67 internal "green" tests represent the outcome of approximately four years of research, coding, and experimentation.
When will Avisna be released to the public?
If the results remain positive, we will announce the initial release upon the completion of version 0.7.
Avisna 以伊朗科学家阿维森纳 (Avicenna) 的名字命名。之所以选择这个名字，是为了用一个恰当的名称来代表智慧、启蒙和科学，从而打造出这门专门用于机器人领域的语言。

Avisna 的编译器并非从零开始编写，我们也并不声称自己是完全自主开发的。这门语言建立在 R 语言的肩膀上，并努力在此基础上发展壮大。

因此，这门语言类似于一种低级（或中级）R 语言，它直接与硬件交互。尽管如此，它仍然力求使其语法尽可能简单，更接近人类语言。

fn main() -> int {
return 56;

}

为什么不将语言和库分开呢？

这种分离最重要的原因是内存。R 语言依赖于 Burrow Checker 内存，它本身就是内存架构的杰作，而 Avisna 语言则依赖于 Arena Sheard 内存。顾名思义，这种内存依赖于内存分区。

第二个原因是，学习 Avisna 不需要学习任何真正的编程语言，您可以直接学习 Avisna，无需任何编程基础。我们努力将学习曲线控制在平均水平以下。

我们正在开发 06 版本。迄今为止，经过我们近 4 年的研究、编码和测试，已获得 219 项 .avs 代码测试通过和 67 项内部测试通过。

Avisna 何时会向所有人发布？

如果测试结果保持良好，我们将在 07 版本准备就绪时发布首个版本。
