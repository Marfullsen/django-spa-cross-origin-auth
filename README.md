# Django + SPA (cross origin) Auth

Cookie based authentication, frontend and backend separated, cross origin.

Original Article [here](https://testdriven.io/blog/django-spa-auth/)

## Updated

- Bug with obsolete component solved using `--openssl-legacy-provider`
- Clon written with Vue3 added.

## Getting Started

Open a console for the frontend and another one for the backend. **Keep both active to run the project.**

Run Django:

```sh
$ cd backend
$ python -m venv venv && source venv/bin/activate
(venv)$ pip install -r requirements.txt
(venv)$ python manage.py migrate
(venv)$ python manage.py runserver
```

---

Choose between React or Vue for the frontend.

Run React:

```sh
$ cd frontend_react
$ npm install
$ npm start
```

Test at [http://localhost:3000/](http://localhost:3000/).

**or**

Run Vue:

```sh
$ cd frontend_vue
$ npm install
$ npm run dev
```

Test at [http://localhost:5173/](http://localhost:5173/).
