import React, {Component} from 'react';
import {ScrollView, Image} from 'react-native';
import {Container, Header, Button, Content, Text, Thumbnail, Left, List, ListItem, Body, Item, Icon, Input, Footer, FooterTab, Card, CardItem, Right } from 'native-base';
import axios from 'axios'

class App extends Component {
  constructor() {
    super();
    this.state = {
      resto: [],
    }
  }

  search = ''

  cari (){
    var url = 'https://developers.zomato.com/api/v2.1/search?q=' + this.search;
    var config = {
      headers: {'user-key': 'b0608885ae18de6e1a28c3ba4788ae7a'}
    };
    axios.get(url, config)
    .then((ambilData) => {
      this.setState({
        resto: ambilData.data.restaurants,
      })
    })
    console.log(this.search)
  }

  render() {
    const data = this.state.resto.map((item, index) => {
      var nama = item.restaurant.name;
      var kota = item.restaurant.location.city;
      var alamat = item.restaurant.location.address;
      var harga2 = item.restaurant.average_cost_for_two;
      var harga1 = harga2/2;

      return (
        <Card key={index}>
          <CardItem key={index}>
            <Left>
              <Thumbnail square source={{uri: item.restaurant.thumb ? item.restaurant.thumb : "https://tinyurl.com/yaqa8yko"}}/>
              <Body>
                <Text key={index}> {nama} </Text>
                <Text note>kota</Text>
              </Body>
            </Left>
            <Right>
              <Text> {harga1} </Text>
            </Right>
          </CardItem>
          <CardItem>
            <Body>
              <Image source={{uri: item.restaurant.thumb ? item.restaurant.thumb : "https://tinyurl.com/yaqa8yko"}} style={{height:200, width: 370, flex: 1}}/>
            </Body>
          </CardItem>
          <CardItem>
            <Icon name= "home" />
            <Text>{alamat}</Text>
          </CardItem>
        </Card>
      )
    })

    return (
      <Container>
        <Header searchBar rounded>
          <Item>
            <Icon name="search" />
              <Input placeholder="Search Restaurant..." onChangeText={(search) => this.search = search} />
            <Icon name="people" />
          </Item>
        </Header>
        <Header>
          <Button block onPress={() => this.cari(this.search)}>
            <Text>
              Cari Restaurant
            </Text>
          </Button>
        </Header>
        <Content>
          <ScrollView>
            <Text style={{fontSize: 40}}>Daftar Resto</Text>
            <List>
              {data}
            </List>
          </ScrollView>
        </Content>
      </Container>
    )
  }

}

export default App;