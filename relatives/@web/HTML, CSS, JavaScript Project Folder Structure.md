A well-organized folder structure improves code readability, maintainability, and scalability. Below is a recommended structure for a vanilla frontend project.

```plaintext
my-project/
│
├── index.html              # Main HTML file (entry point)
├── about.html              # Other HTML pages
│
├── css/                    # All CSS files
│   ├── style.css           # Main stylesheet
│   └── components/         # (Optional) Reusable styles
│       └── navbar.css
│
├── js/                     # JavaScript files
│   ├── main.js             # Main script
│   └── utils/              # (Optional) Utility scripts
│       └── helper.js
│
├── assets/                 # Static assets
│   ├── images/             # Images
│   │   └── logo.png
│   ├── fonts/              # Custom fonts
│   └── icons/              # Icons
│
├── libs/                   # Third-party libraries (optional)
│   └── jquery.min.js
│
├── data/                   # (Optional) JSON or mock data
│   └── data.json
│
├── README.md               # Project documentation
└── LICENSE                 # Project license
```

## Folder & File Descriptions
| Folder/File  | Description                               |
| ------------ | ----------------------------------------- |
| `index.html` | The main entry point HTML file            |
| `css/`       | Contains all styling files                |
| `js/`        | All JavaScript logic/scripts              |
| `assets/`    | Images, fonts, icons, etc.                |
| `libs/`      | External libraries/plugins (not from CDN) |
| `data/`      | JSON or mock data files                   |
| `README.md`  | Overview and instructions for the project |
| `LICENSE`    | Open-source license file                  |

## Tips

- Use **semantic and meaningful names** for files and folders.
    
- Keep styles, scripts, and assets **modular and reusable**.
    
- Group similar types of files together.
    
- Use a **version control system** (e.g., Git) and include a `.gitignore`.