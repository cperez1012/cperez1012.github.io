---
layout: post
title:      "React Redux Project Overview - Issues and Solutions"
date:       2020-11-22 18:32:24 +0000
permalink:  react_redux_project_overview_-_issues_and_solutions
---


I'm happy to say that my journey as a student has come to an end. I have learned so much while being a part of the Flatiron community. From starting off learning the basics of Ruby to building a full fledge web application that people can use! In this blog I want to go over the issues I had came across when working on my final portfolio project and the solutions to those issues. My web application is named ***The Boxing Fan App*** . My web application was created using React Redux on the frontend and Rails on the backend.  ***The Boxing Fan App***  functions with a user creating an account, creating a list (i.e "Greatest Fighters of All Time"), and then the user can create a fighter to add to their list and/or update the fighter's information so that it's the most up-to-date.

# **Serializers**
My first issue was trying to properly reflect the associated attributes for my fighters on a list. With just the serializer for my list, I could only see on my rails server the relationship to the fighters and within that relationship was just the fighter id's and type called "fighter". In order for me to access the fighters attributes, I added :fighters to my list serializer file and after doing so, I saw all the information I needed to show on my List's page that has the fighters on that page. After doing this I decided to do the same within the Fighter's page and have the associated list's name availble for the user to see.

# **Frontend Form**
My second issue was trying to define my handleChange hook to take into account my checkbox for my "Check if they were a Champion" section on my form while also deal with updating the name and value for the rest of my form. Luckily I was able to use a ternary operator to set the conditons that if its a checkbox then the form will update accordingly to the checkbox field and if otherwise then the other type of values on my form will update accordingly. 

```
    const handleChange = event => {

        const target = event.target
        const value = target.type === 'checkbox' ? target.checked : target.value;
        const name = target.name
        
        updateNewFighterForm(name, value)
    }
```

# **Reducers**
My last issue was acknowledging the importance of correctly naming the case wihtin my reducer to match what I have defined in my actions file. For example, in my fighters reducer I named my case "ADD_FIGHTERS": and in my figher's action file I defined my addFighter constant  type as "ADD_FIGHTER". In doing so I was unable to properly add fighter's to my user's database. After the help from my fellow Flatiron colleague Steven, he pointed out that I need to be careful when naming the case in my reducer to match the type in my action's file or else my application would not know what needs to be done.

# **Conclusion**
In conclusion, I'm glad I'm done with my last project but also glad I went throught the headaches with my project because I feel like I gained a much better understanding of how a web application should work on both my backend and frontend. Please checkout my frontend and backend repos below to enjoy using my web application.

