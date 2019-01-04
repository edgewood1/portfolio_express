# portfolio

heroku info -s | grep web_url | cut -d= -f2

https://evening-atoll-18606.herokuapp.com/

git push heroku master

components

1. blog -- shows all the categories
    / Link sends key

2. react_blog - if you click on react, goes to iframe to show jekyl
    ---> instead, api call to github and grabs all markdown titles
2. all_posts - if you click on any other wordpress category
    // const {id} = this.props.match.params; - used to get key
    // link sends

3. one_post - shows an individual wordpress post -- could this also show

