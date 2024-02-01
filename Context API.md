
```js
// UserContext.js

import React from 'react'
const UserContext = React.createContext()
export default UserContext;

```

```js
// UserContextProvider.jsx

import React from "react";
import UserContext from "./UserContext";

const UserContextProvider = ({children}) => {
    const [user, setUser] = React.useState(null)
    return(
        <UserContext.Provider value={{user, setUser}}>
	        {children}
        </UserContext.Provider>
    )
}

export default UserContextProvider
```

<div align='center'>OR</div>

```js
//theme.js

import { createContext, useContext } from "react";

export const ThemeContext = createContext({
    themeMode: "light",
    darkTheme: () => {},
    lightTheme: () => {},
})

export const ThemeProvider = ThemeContext.Provider

export default function useTheme(){
    return useContext(ThemeContext)
}
```

```js
// App.jsx


import { useEffect, useState } from 'react'
import { ThemeProvider } from './contexts/theme'

function App() {
  const [themeMode, setThemeMode] = useState("light")

  const lightTheme = () => {
    setThemeMode("light")
  }

  const darkTheme = () => {
    setThemeMode("dark")
  }

  useEffect(() => {
    document.querySelector('html').classList.remove("light", "dark")
    document.querySelector('html').classList.add(themeMode)
  }, [themeMode])
  

  return (
    <ThemeProvider value={{themeMode, lightTheme, darkTheme}}>
	    <h1>Context API</h1>
    </ThemeProvider>
  )
}
export default App

```

- Now in the toggle theme button we can add the functionality to toggle the modes and it will be applied to the whole app.
- The `lightTheme` and `darkTheme` is not assigned any task, that will be done in the `<Button />` component.
- We can do the same in this file even. Below is the example illustrating the same example.


> An example from SMS

```js
import { createContext, useContext, useEffect, useState, ReactNode } from 'react';
import { createContext, useContext, useEffect, useState, ReactNode } from 'react';
import { baseUrl } from '../API/api';

const AuthContext = createContext({
  token: null,
  login: () => {},
  logout: () => {},
  type: null,
  isUserLoggedIn: false,
});

const checkTokenValidity = async (token, setType, setToken) => {
  try {
    const response = await fetch(`${baseUrl}/auth/verifytoken`, {
      method: 'GET',
      headers: {
        Authorization: `Bearer ${token}`,
      },
    });

    if (!response.ok) {
      throw new Error(`Token verification failed: ${response.statusText}`);
    }
    if (response.ok) {
      const data = await response.json();
      setType(data.type);
      setToken(token);
    } 
  } catch (error) {
    console.error('Error during token verification:', error);
  }
};


export const AuthProvider = ({ children }) => {
  const [token, setToken] = useState(null);
  const [type, setType] = useState(null)

  useEffect(() => {
    const storedToken = localStorage.getItem('token');


    if (storedToken) {
      checkTokenValidity(storedToken, setType, setToken);
    }
  }, []);

  const loginHandler = (token) => {
    setToken(token);

    localStorage.setItem('token', token);
  };

  const logoutHandler = () => {
    setToken(null);

    localStorage.removeItem('token');
  };

  const isUserLoggedIn = () => {
    return !!token;
  };

  const authContextValue = {
    token: token,
    login: loginHandler,
    logout: logoutHandler,
    type: type,
    isUserLoggedIn: isUserLoggedIn(),
  };

  return (
    <AuthContext.Provider value={authContextValue}>{children}</AuthContext.Provider>
  );
};

AuthProvider.propTypes = {
  children: ReactNode
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
const AuthContext = createContext({
token: null,
login: () => {},
logout: () => {},
type: null,
isUserLoggedIn: false,
});

const checkTokenValidity = async (token, setType, setToken) => {
	// Function to validate token from backend
};

export const AuthProvider = ({ children }) => {
	const [token, setToken] = useState(null);
	const [type, setType] = useState(null)
	
	useEffect(() => {
		const storedToken = localStorage.getItem('token');
		if (storedToken) {
			checkTokenValidity(storedToken, setType, setToken);
		}
	}, []);
	
	const loginHandler = (token) => {
		setToken(token);
		localStorage.setItem('token', token);
	};
	
	const logoutHandler = () => {
		setToken(null);
		localStorage.removeItem('token');
	};
	
	const isUserLoggedIn = () => {
		return !!token;
	};
	
	const authContextValue = {
		token: token,
		login: loginHandler,
		logout: logoutHandler,
		type: type,
		isUserLoggedIn: isUserLoggedIn(),
	};
	  
	return (
	<AuthContext.Provider value={authContextValue}>
		{children}
	</AuthContext.Provider>
	)
};

AuthProvider.propTypes = {
	children: ReactNode
};

export const useAuth = () => {
	const context = useContext(AuthContext);
	if (!context) {
		throw new Error('useAuth must be used within an AuthProvider');
	}
	return context;
};
```