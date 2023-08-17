# SpaceCAT

## Description
This was my third project overall, and first group project completed as part of the General Assembly Software Engineering Bootcamp Course. The project we worked on is a website named SpaceCAT, which allows users to view, create and add to an album as well as modify albums of NASA’s Astronomy Picture of the Day (APOD).
SpaceCAT was built by me, Trung, along with my fellow aspiring software engineers, Amrit and Costa! We were inspired to create this app because we wanted to expand our knowledge about space and discover the wonders that lie beyond our planet. An interesting coincidence is that the initials of our first names, C, A, and T, led us to the perfect name for our app: SpaceCAT!

## Deployment link
The project is hosted [here](https://space-cat.fly.dev/).

## Getting Started/Code Installation

1. **Clone the repository**:
    `git clone https://github.com/AmritpalC/SpaceCAT`
2. **Setup Environment Variables**:
    Create .env file and get NASA APOD API with geolocation API from api.ipgeolocation.io

    - APOD_KEY: This variable holds the API key required for accessing the Astronomy Picture of the Day (APOD) service.
    - ROOT_URL: This variable defines the root URL of the application or service.
    - API_KEY: This variable stores a general API key, potentially used for other API interactions within the project.

3. **Run Commands**
    - python manage.py createsuperuser
    - python manage.py runserver 

    Website should be on your local browser at `http://localhost:8000/`

## Timeframe & Working Team 

It was a group project and we were given 1 week to start and finish the project.

## Technologies Used
This project required Neon, SQL, HTML, CSS and Python.

Frameworks used: 
- Bootstrap 
- Django 
- Third party APIs from NASA and IP Geolocation

## Brief
Your App Must:

- Be a full-stack Django application.
- Connect to and perform data operations on a PostgreSQL database (the default SQLLite3 database is not acceptable).
- If consuming an API (OPTIONAL), have at least one data entity (Model) in addition to the built-in User model. The related entity can be either a one-to-many (1:M) or a many-to-many (M:M) relationship.
- If not consuming an API, have at least two data entities (Models) in addition to the built-in User model. It is preferable to have at least one one-to-many (1:M) and one many-to-many (M:M) relationship between entities/models.
- Have full-CRUD data operations across any combination of the app's models (excluding the User model). For example, creating/reading/updating posts and creating/deleting comments qualifies as full-CRUD data operations.
- Authenticate users using Django's built-in authentication.
- Implement authorization by restricting access to the Creation, Updating & Deletion of data resources using the login_required decorator in the case of view functions; or, in the case of class-based views, inheriting from the LoginRequiredMixin class.
- Be deployed online using Heroku. Presentations must use the deployed application.

Full requirements: <a href="https://git.generalassemb.ly/SEI-72-LDN/SEIR-Courses-Materials/blob/main/Unit_3/project-3/project-3-requirements.md" target="_blank" rel="noopener noreferrer">here</a>.

## Planning

For planning we used Trello board and Excalidraw.
<img src="https://i.imgur.com/SBADSWC.png" alt=""/>

<img src="https://i.imgur.com/pLFqbAF.png" alt=""/>

Full Trello board: [here](https://trello.com/b/uM4ikbkh/project-3-full-stack-django-app).

## Build/Code Process

Through a Zoom meeting, we collaboratively assigned tasks, with my focus primarily on CSS while Amrit and Costa concentrated on the backend aspects. Our initial steps included establishing a GitHub repository and leveraging the login and signup functionality code demonstrated during our class sessions.

We started by creating models
```python
class Apod(models.Model):
title = models.CharField(max_length=100, default='APOD')
url = models.URLField(default='')
date = models.DateField()
explanation = models.TextField(max_length=3000)
users = models.ManyToManyField(User, related_name='apods')
def __str__(self):
return self.title

class Meta:
ordering = ['title']

class Album(models.Model):
name = models.CharField(max_length=100)
description = models.TextField(max_length=500)
apod_photos = models.ManyToManyField(Apod)
user = models.ForeignKey(User, on_delete=models.CASCADE)
```

The Apod model represents Astronomy Picture of the Day entries, including fields like title, url, date, and explanation. It establishes a many-to-many relationship with users. The Album model represents albums containing APOD photos. It includes attributes such as name, description, and many-to-many relationships with APOD entries. This model is linked to the User model through a foreign key. The Satellite model describes satellites with attributes such as name, type, and description.

Following this, I directed my efforts towards implementing the Time and Location API, a component originally intended for satellite tracking functionality. However, during the development process, its application was streamlined to exclusively enhance the home page experience.
```python
dt_key = env('API_KEY')
def home(request):
# Current Time and Location api
url = f'https://api.ipgeolocation.io/ipgeo?apiKey={dt_key}'
response = requests.get(url)
data = response.json()

current_datetime = data['time_zone']['current_time']
location = data['country_code2']
city = data['city']

context = {
'current_datetime': current_datetime.split('.')[0],
'location': location,
'city': city
}
```

The code begins by importing required modules, including requests for making HTTP requests and decouple for managing environment variables. An API key (dt_key) is retrieved from the environment using the config function from decouple. The home view function is defined. Inside this function, an API request is made to the IP Geolocation API using the provided API key. The response is then parsed as JSON to extract relevant data such as the current date and time (current_datetime), the country code (location), and the city (city). A context dictionary is prepared with this extracted data, which will be passed to the template for rendering. Finally, the home template is rendered with the retrieved data, creating a dynamic experience for the user by displaying the current time, location, and city.

Upon successfully implementing this backend code segment, my focus shifted towards enhancing the website's aesthetics and user interface. I employed SCSS (Sass) with the Bootstrap framework to achieve this goal. SCSS, a CSS preprocessor, offered a more efficient and organized approach to styling by allowing the use of variables, nested rules, and other advanced features. Bootstrap, a widely-used front-end framework, provided a range of responsive design components and utilities, ensuring a polished and visually appealing user experience. This phase of the project involved fine-tuning layout, color schemes, and responsiveness to create a visually engaging and user-friendly website interface.
```scss
#wrap {
min-height: 90%;
.container {
overflow: auto;
padding-bottom: 20px;


.card-album {
max-width: 350px;
width: 100%;
height: 100px;
margin: 10px auto;
background-color: grey;
border-radius: 30px;
border: 2px solid rgb(255, 125, 72);
&:hover {
background-color: darkgrey;
color: white;
}
&:focus {
background-color: darkgrey;
color: white;
}
.album {
text-align: center;
line-height: 100px;
a {
text-decoration: none;
display: block;
font-size: 24px;
font-weight: bold;
color: rgb(12, 0, 46);
&:hover {
color: #ffffff;
}}}}}}
```
## Challenges

In this project's creative phase, I encountered intriguing challenges. The lack of a fixed idea at the start led to uncertainty, yet it also encouraged a more open and innovative approach. Navigating this uncertainty sparked dynamic exploration, resulting in an evolving design from initial wireframes to the final product. Embracing this adaptability contributed to the project's uniqueness, highlighting the value of flexibility and open-mindedness in creative work.

## Wins

Engaging in group collaboration has yielded numerous wins, and I take pride in effectively leveraging Python, SCSS, and Bootstrap. Diverse perspectives led to innovative solutions, while shared workload and synergy enhanced efficiency and quality. Learning from peers boosted my skills and insights. The motivation and accountability within the group boosted my productivity. These experiences in group work, combined with utilising Python, SCSS, and Bootstrap, have been fulfilling.

## Key Learnings/Takeaways

Enhanced Teamwork and Technical Skills: The project was instrumental in advancing both my teamwork and technical capabilities. 
Proficiency in Django and Python: I gained valuable experience in developing projects using Django and Python. 
Leveraging Bootstrap and Sass: With the support of Bootstrap and Sass frameworks, I deepened my understanding of creating web applications. 
Group Collaboration Insights: The project underscored the significance of group collaboration, enabling efficient workload division and completion of specific web application components.

## Bugs

After deploying the location and date API is not taking current location and time of the user, but from the server that is in USA. Therefore, it is displaying incorrect time/location.

## Future Improvements

Improving Homepage Design: Enhancing the homepage design posed a challenge as it involved dynamically displaying NASA's Astronomy Picture of the Day, which varied in size and colors. To address this we could add space themed fillers. 
Resolving Location and Time API Issue: Following deployment, I observed that the location and time API displayed the server's location instead of the user's.
Adding Functionality: Introducing satellite tracking, that can be added to the album by users. 


