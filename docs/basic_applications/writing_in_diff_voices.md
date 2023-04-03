---
sidebar_position: 60
---

# 🟢 不同的写作风格

像[ChatGPT](https://chat.openai.com/chat)这样的人工智能可以在一瞬间撰写论文和博客文章。但是通常，它们产生的文章都很普通、枯燥。您需要做一些额外的工作，让文字更有活力或个性。其中最快的方法之一是指定一种写作风格或在提示中命名著名的作者。

假设您想要一段关于友谊重要性的内容。一个通用的提示，“写一段关于友谊重要性的内容”，将会产生一个平淡无奇的回应。像下面这样：

import friendship from '../assets/basic_applications/Generic_Friendship.png';
import friendship_informal from '../assets/basic_applications/Friendship_Informal.png';
import friendship_mark from '../assets/basic_applications/Friendship_Mark_Twain.png';
import friendship_chris from '../assets/basic_applications/Friendship_Chris_Rock.png';
import friendship_millennial from '../assets/basic_applications/Friendship_Millennial.png';


<div style={{textAlign: 'center'}}>
  <img src={friendship} style={{width: "750px"}} />
</div>

嗨！太单调了。加上注明“**请以非正式、对话式的风格写作**”等规范之后，ChatGPT会生成下面这样的内容：

<div style={{textAlign: 'center'}}>
  <img src={friendship_informal} style={{width: "750px"}} />
</div>

好多了。

而如果你想让ChatGPT用一位文学巨匠的风格进行写作，如马克·吐温的风格：

<div style={{textAlign: 'center'}}>
  <img src={friendship_mark} style={{width: "750px"}} />
</div>

现在的文字开始唱起来了。但不一定非得是著名的作者。您还可以使用知名的个人，比如Chris Rock。

<div style={{textAlign: 'center'}}>
  <img src={friendship_chris} style={{width: "750px"}} />
</div>

或者那些“千禧一代”呢？

<div style={{textAlign: 'center'}}>
  <img src={friendship_millennial} style={{width: "750px"}} />
</div>

## 复制您的写作风格


如果您希望GPT-3以**您的**风格编写电子邮件或博客呢？

只需向它展示您过去撰写的一些内容即可。以下是一个例子：我们向GPT-3展示了“Rodriguez博士”过去写过的一些电子邮件，并要求他用相同的风格给新病人Inuez先生写一封新的电子邮件。

<div trydyno-embed="" openai-model="text-davinci-003" initial-prompt="邮件#1: 主题-预约确认\n\n亲爱的John，\n\n我想确认您与我，Smith博士，于2023年5月1日下午2:30在HealthFirst诊所的预约。我期待着与您见面，讨论您的健康问题。\n\n请提前15分钟到达，填写任何必要的文件，并带上您目前正在服用的任何药物清单。如果您需要重新安排或取消您的预约，请至少提前24小时通知我们。\n\n如果您在预约之前有任何疑问或担忧，请不要犹豫与我们联系。\n\n此致\n敬礼，\nRodriguez博士\n\n邮件#2:主题-检查结果\n\n亲爱的Jane，\n\n我写信是想让您知道，您最近的血液检测结果已出来。您的胆固醇水平在正常范围内，但您的维生素D水平较低。我建议您开始服用维生素D补充剂以改善水平。\n\n基于这些结果，我还建议您在三个月后安排一次随访检查，以重新测试您的维生素D水平。如果您有任何问题或疑虑，请不要犹豫与我们联系。\n\n谢谢您，\nRodriguez博士\n\n邮件#3:主题-随访预约\n\n亲爱的马克，\n\n我想在我们上次约会后跟进一下，看看您的感觉如何。如果您仍然感到背痛或有任何新的担忧，请告诉我，以便我们在即将到来的约会期间讨论。\n\n您的下一次约会已定于2023年6月15日上午10:00在City Health Clinic举行。如果您需要重新安排或取消，请告知我们。\n\n保重，\nRodriguez博士\n\n按照以上电子邮件的风格向Inuez先生写一封邮件，询问他10月5日是否适合检查预约：" initial-response="主题-检查预约\n\n尊敬的Inuez先生，\n\n我希望您过得愉快。我写信是为了询问您10月5日是否适合与我，Rodriguez博士进行检查预约。\n\n在预约期间，我们将讨论您的健康问题和医疗历史的任何变化。请提前15分钟到达，填写任何必要的文件，并带上您目前正在服用的任何药物清单。\n\n如果10月5日不适合您，请告诉我，我们可以找到一个更适合您的日期。\n\n谢谢您，\nRodriguez博士" max-tokens="256" box-rows="18" model-temp="0" top-p="0">
    <noscript>加载失败：必须启用JavaScript</noscript>
</div>
 

<br/>您可以输入您过去撰写的任何文本（电子邮件、报告、博客等），GPT-3/ChatGPT通常都能够复制您的风格。您输入的文本越多，它复制您的风格就会越好。