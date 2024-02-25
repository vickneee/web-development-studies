# Using Local Database in React Components

In this example:

- We iterate over each skillsSet in the skills array using the map function, providing a unique setIndex for each set.
- Inside each skillsSet, we use another map function to iterate over the skillsList array, providing a unique skillIndex for each skill within the set.
- Each skill is then rendered with its corresponding data, including the link, imgSrc, imgAltText, and skillName.

**Skills-DB.js:**

```javascript
import SUBLIMETEXT from "../../assets/img/skills/sublime-text.png"
import VISUAL_STUDIO_CODE from "../../assets/img/skills/vscode.png"

// Skills data
export const skills = {
    skillsList: [
        {
            link: "https://www.sublimetext.com/",
            imgAltText: "Sublime Text",
            imgSrc: SUBLIMETEXT,
            skillName: "Sublime Text",
        },
        {
            link: "https://code.visualstudio.com/",
            imgAltText: "Visual Studio Code",
            imgSrc: VISUAL_STUDIO_CODE,
            skillName: "Visual Studio Code",
        },

    ],
};
```

**Skills.js**

```javascript
import {skills} from "./Skills-DB";

const Skills = () => {
    return (
        <div className="flex justify-evenly h-full m-auto">
            <div className="p-40" id="skills">
                <h1 className="pb-3 text-7xl">Tools and Tech Skills</h1>
                <div className="flex shadow-md justify-evenly">
                        <div className="flex p-20 gap-40">
                            {skills.skillsList.map((skill, index) => (
                                <span key={index}>
                                        <a href={skill.link}
                                           target="_blank" rel="noopener noreferrer">
                                            <img src={skill.imgSrc} alt={skill.imgAltText} className="h-20 pb-8"></img>
                                        </a>
                                    </span>
                            ))}
                        </div>
                    </div>
                </div>

        </div>
    );
};

export default Skills;
```



