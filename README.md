# Fluidcoins Component for Vue
A Vue Plugin for Fluidcoins payment button.

## Installation
``` 
    npm install vue vue-fluidcoins --save 

    # OR
    
    yarn add vue vue-fluidcoins 

```

### Usage
```
    <fluidcoins 
        type="secondary"
        rounded
        publicKey="PUBLIC_KEY"
        :amount="90000"
        email="myemail.com"
        :on-success="onSuccess"
    />
```
Please checkout [Fluidcoins' Documentation](https://developers.fluidcoins.com/docs/overview) for other options to help you accept crypto payments online easily.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE) file for details
