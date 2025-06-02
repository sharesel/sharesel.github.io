// Modern Cybersecurity Portfolio using React + TailwindCSS + Bootstrap
// Built for GitHub Pages deployment

import React, { useState } from "react";
import { projects, getCategoryProjects } from "./data/projects";
import { Moon, Sun } from "lucide-react";

export default function Portfolio() {
  const [darkMode, setDarkMode] = useState(false);
  const [selectedProject, setSelectedProject] = useState(null);
  const [activeCategory, setActiveCategory] = useState("all");

  const filteredProjects =
    activeCategory === "all"
      ? projects
      : getCategoryProjects(activeCategory);

  const toggleTheme = () => setDarkMode(!darkMode);

  return (
    <div className={darkMode ? "dark bg-gray-900 text-white" : "bg-white text-black"}>
      {/* Navbar */}
      <nav className="flex justify-between items-center p-4 border-b border-gray-200 dark:border-gray-700">
        <h1 className="text-2xl font-bold">Cybersecurity Portfolio</h1>
        <ul className="flex gap-4">
          <li><a href="#home">Home</a></li>
          <li><a href="#projects">Projects</a></li>
          <li><a href="/resume.pdf" target="_blank">Resume</a></li>
          <li><a href="#contact">Contact</a></li>
        </ul>
        <button onClick={toggleTheme} className="ml-4 p-2 rounded">
          {darkMode ? <Sun /> : <Moon />}
        </button>
      </nav>

      {/* Category Filters */}
      <section className="p-4 text-center" id="projects">
        <div className="flex justify-center flex-wrap gap-4 mb-6">
          {["all", "cloud", "ai", "systems", "linux", "windows", "crypto"].map(cat => (
            <button
              key={cat}
              onClick={() => setActiveCategory(cat)}
              className={`px-4 py-2 rounded-full border ${
                activeCategory === cat
                  ? "bg-blue-600 text-white"
                  : "bg-transparent border-blue-600 text-blue-600"
              }`}
            >
              {cat.toUpperCase()}
            </button>
          ))}
        </div>

        {/* Project Grid */}
        <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
          {filteredProjects.map((proj) => (
            <div
              key={proj.id}
              className="bg-white dark:bg-gray-800 shadow-md rounded-xl overflow-hidden cursor-pointer border dark:border-gray-700"
              onClick={() => setSelectedProject(proj)}
            >
              <img src={proj.image} alt={proj.title} className="w-full h-48 object-cover" />
              <div className="p-4">
                <h3 className="text-lg font-semibold">{proj.title}</h3>
                <p className="text-sm text-gray-600 dark:text-gray-300">{proj.description}</p>
              </div>
            </div>
          ))}
        </div>
      </section>

      {/* Modal */}
      {selectedProject && (
        <div className="fixed inset-0 z-50 bg-black bg-opacity-70 flex items-center justify-center p-6">
          <div className="bg-white dark:bg-gray-900 p-6 rounded-lg max-w-3xl w-full overflow-y-auto max-h-[90vh]">
            <button
              className="absolute top-4 right-4 text-xl font-bold"
              onClick={() => setSelectedProject(null)}
            >
              ×
            </button>
            <h2 className="text-2xl font-bold mb-2">{selectedProject.title}</h2>
            <p className="text-sm text-gray-600 dark:text-gray-400 mb-4">{selectedProject.details}</p>
            {selectedProject.hasVideo && selectedProject.videoUrl && (
              <video src={selectedProject.videoUrl} controls className="w-full rounded-md mb-4" />
            )}
            {selectedProject.hasScreenshots && selectedProject.screenshots && (
              <div className="grid grid-cols-2 gap-2">
                {selectedProject.screenshots.map((src, idx) => (
                  <img
                    key={idx}
                    src={src}
                    alt={`Screenshot ${idx + 1}`}
                    className="rounded border"
                  />
                ))}
              </div>
            )}
          </div>
        </div>
      )}

      {/* Contact Footer */}
      <footer id="contact" className="p-6 text-center mt-12 border-t dark:border-gray-700">
        <p>Connect with me: <a href="mailto:your@email.com" className="text-blue-600">Email</a> | <a href="https://linkedin.com/in/yourprofile" className="text-blue-600">LinkedIn</a></p>
        <p className="mt-2 text-sm text-gray-500">© {new Date().getFullYear()} Your Name</p>
      </footer>
    </div>
  );
}

// Note: Also create a `data/projects.ts` file using your provided array of projects
// Then build and deploy this with `npm run build` and push to GitHub for GitHub Pages
