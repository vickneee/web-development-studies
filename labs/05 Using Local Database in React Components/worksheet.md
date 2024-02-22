# Using Local Database in React Components

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
            imgSrc: "sublime-text.png",
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
import React from "react";
import { skills } from "./Skills-DB";

const Skills = () => {
    return (
        <div className="skills">
            {skills.skillList.map((skillsSet, setIndex) => (
                <div className="skills-set" key={setIndex}>
                    <h1>Skills Set {setIndex + 1}</h1>
                    <div className="skills-list">
                        {skillsSet.skillsList.map((skill, skillIndex) => (
                            <div className="skill" key={skillIndex}>
                                <h3>{skill.skillName}</h3>
                                <a href={skill.link} target="_blank" rel="noopener noreferrer">
                                    <img src={skill.imgSrc} alt={skill.imgAltText} />
                                </a>
                            </div>
                        ))}
                    </div>
                </div>
            ))}
        </div>
    );
};

export default Skills;
```

In this example:

- We iterate over each skillsSet in the skills array using the map function, providing a unique setIndex for each set.
- Inside each skillsSet, we use another map function to iterate over the skillsList array, providing a unique skillIndex for each skill within the set.
- Each skill is then rendered with its corresponding data, including the link, imgSrc, imgAltText, and skillName.

