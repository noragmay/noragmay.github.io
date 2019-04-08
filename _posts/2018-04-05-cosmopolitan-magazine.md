---
layout: post
title: Cosmopolitan Magazine: A Data-Driven Historical Analysis
---

**Natural Language Processing and image processing to analyze trends on Cosmopolitan Magazine covers**

Magazine covers have an underrated influence on our culture. Even if you aren’t reading the tabloids or women’s magazines, if I tell you to picture a Cosmo or a People, you can picture the cover and give some examples of the stories they run (“Sexy Hair: Fresh Looks You’ll Adore”). This is because every time you’ve purchased groceries or gotten a snack from the plane, you see magazines. While you wait in line you read the sensationalized headlines and maybe, every once and a while, you even purchase one (just me? Okay.)

To try to further understand this potential influencer of our culture, I decided to extract the text from women’s magazine covers, perform NLP topic modeling, and use image processing techniques to understand graphic trends and representation. I wanted to analyze how their intention to appeal to women migrated from this:

![Old Covers]({{ site.url }}/images/OldCover.jpg)

to this:

![New Covers]({{ site.url }}/images/NewCover.jpg)

I entered into this investigation with a few main questions: first, does this data source exist? I wasn’t sure if anyone had even collected all covers of a women’s magazine digitally. Luckily, the academics had me covered. There is a Proquest database called the Women’s Magazine Archive that has covers of various magazines, including Cosmo from 1925-2005, which is how I landed on this specific publication. I ended up using covers from 1950-2005 from this database and individually searching for and downloading covers for 2006-2019.

My second question was “can I use computer vision techniques to record the text off of the .jpg files.” This had a more complicated answer:

I used Google’s tesseract optical character recognition (OCR) engine, an open source tool trained to recognize and extract text. Tesseract worked well on covers up until the 90s, with a single background color and text color, because the process uses binarization, bringing the pixel values above a threshold to white, and below to black.

![1970]({{ site.url }}/images/1970.png)

Later covers with more sophisticated graphics meant tesseract ocr’s default process lost more than half the text. To solve this, I inverted the images and changed the binarization threshold to be between the background color and the lighter text colors and was able to extract a larger percentage of the text.

![eva]({{ site.url }}/images/eva.png)

However, I used a standard pixel value for all covers as the binarization threshold, I think that this would have a higher retention rate with a dynamic threshold that varied based on the background and text colors, a space for further work.

My last question was “what information can I extract using data science processing techniques that go beyond what I can generalize just looking at some example covers?” Leveraging natural language processing, clustering algorithms, and composite images, the answer was quite a lot. 

With the recorded cover text, I performed natural language processing. Using NMF topic modeling, I saw these 6 topics over the 70 years of covers:

<table>
  <thead>
    <tr>
      <th>Topic Name</th>
      <th>Most Common Words </th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Sex:</td>
      <td>make ways life sexy ever good bed hot know tricks things secret guys never feel</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Mysteries:</td>
      <td>complete mystery murder husband see stories girls short money summer suspense children special</td>
    </tr>
    <tr>
      <td>Fiction:</td>
      <td>best girl plus short stories girls story fiction life seller book diet two lover woman</td>
    </tr>
    <tr>
      <td>Resolutions:</td>
      <td>astrologer bedside money career guide cosmos health bonus star year plus beauty travel advice family</td>
    <tr>
    </tr>
      <td>Relationships/Life:</td>
      <td>life super way rich much affair know want quiz long relationship young</td>
    </tr>
   	<tr>
      <td>Miscellaneous:</td>
      <td>first excerpt big girl together lovers making stunning turn marriage sexual girls romance</td>
    </tr>
  </tbody>
</table>

Surprisingly two out of six topics were associated with literature: fiction and mysteries, which seemed like a large percentage, based on my perception of Cosmo in my lifetime. However, in this process, I learned that the magazine serialized novels and short stories up into the 50s, and fiction continued to appear into the 90s.

![fiction]({{ site.url }}/images/fiction.png)

As you can see on the graph below, which is of the percent of words on the cover in each topic averaged per year. The literature topics continuously decreased during the time period of covers I analyzed.

![topics]({{ site.url }}/images/topiclines.png)

The vertical lines here denote changes in editors. Clearly, topics covered change when editors change. With the first female editor in 1965, Helen Gurley Brown, there is a decrease in literary topics, and in turn you get more about women’s individual lives. When Brown’s 20-year tenure ended. the shift away from literary content stepped up again to an extremely high percentage of words in the “sex” topic.

To understand more about these topics after the era of literature publication, I performed NMF on the text from 1980-2018 and found these topics.

<table>
  <thead>
    <tr>
      <th>Topic Name</th>
      <th>Most Common Words </th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <td>Sex:</td>
      <td>hot bed sexy guys never things moves guy really naughty</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Astrology:</td>
      <td>astrologer bedside cosmos health career guide astral bonus money star</td>
    </tr>
    <tr>
      <td>Relationships:</td>
      <td>man woman girl husbands tantalizing makeup married friends woman secrets marriage</td>
    </tr>
    <tr>
      <td>Cover Girl:</td>
      <td>money big me work hollywood quiz hot good sexy young</td>
    <tr>
    </tr>
      <td>Secrets:</td>
      <td>abad know secrets surprising couples reveal boyfriend terrace readers believe</td>
    </tr>
   	<tr>
      <td>Miscellaneous:</td>
      <td>read things movie keep beautiful working fast bitchy husbands discover</td>
    </tr>
    <tr>
      <td>Hints/How-to:</td>
      <td>guide interested girls know everything relationships christmas marriage survival</td>
    </tr>
    <tr>
      <td>Power:</td>
      <td>best ahead getting scary power years sexy turning learn loving</td>
    </tr>
    <tr>
      <td>Human Interest:</td>
      <td>happen know girl help even rape living sure perhaps stories someone</td>
    </tr>
    <tr>
      <td>Health/Beauty:</td>
      <td>feel tricks secret take story diet stay skin sexier fearless tips beauty</td>
    </tr>
  </tbody>
</table>


If we look at a percentage of words in each of these topics on covers we can see that the 2000s showed a decrease in the “Relationships,” topic reflecting a societal shift away from defining women via boyfriends and marriage.

![topics]({{ site.url }}/images/relationships.png)

At the same time, the health/beauty and sex categories rose, and we also see an increase in the percentage of “power” words, indicating an editorial shift toward empowerment, away from condescending topics like “how-to guides".

![topics]({{ site.url }}/images/power.png)

Finally, I also did image analysis to further support my textual analysis. Using average pixel images I created a composite cover for each decade, and the 50s and 60s did not have a cohesive style, see the 1962 and 1964 images, besides the Cosmo logo, But If I divided the composite into before and after 1965, I found that I could see a portrait start to appear, shifting to a single "cover girl," further supporting my findings that Cosmo's content in the late 60s shifted to focus on the woman as an individual and her particular interests.

![topics]({{ site.url }}/images/60s.png)

Then starting in the 70s we see the standard cover consisting of a central woman usually white posing with 3/4 of her body shown.

![topics]({{ site.url }}/images/70s.png)

With the more contemporary topic changes to more empowering language, I am interested to see how much longer this style exists, because to me it feels a bit more objectifying than empowering, and out of curiosity I looked at the most recent covers at the time of this analysis:

![topics]({{ site.url }}/images/modern.png)

These are March and April’s covers from this year (2019). We have a pizza front and center, and the first cover with multiple people since the 60s, so it does seem that the mold is starting to break down.

In using data to understand text and graphic changes, I was more able to answer my final question:

Are magazines like Cosmo a reflection of changes in attitudes towards women or an influencer thereof?

The answer is likely both. We can see the changes in Cosmo’s topics correlate highly with the different waves of feminism [show that here] and likely it played a part in making sexual empowerment mainstream, but if the society were not already moving in that direction, then the magazines wouldn’t sell. I think that they want to be seen as pushing the boundaries and empowering women, but not to a level that is close to the most progressive feminist thinkers.

Overall, my takeaway was that we are continuing to see growth and changes societies acceptance and support of the autonomy and individuality of women in the material that is meant to attract and portray us.