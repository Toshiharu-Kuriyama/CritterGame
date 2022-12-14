# CritterGame

#include <iostream>
using namespace std;

class Critter {
public:
  int count = 0;
  bool oneTime = true;
  Critter(int hunger = 0, int boredom = 0, int level = 1,int lvlPoint = 0,int incrementNum = 0); // constructor prototype
  void Talk();
  void Eat(int food = 4);
  void Play(int fun = 4);
  void LevelUpDown();//レベルを上下させる関数
  void GetValues() const;
  void Medical();
  void SupplyCapaBoost();


private:
  int m_Hunger;
  int m_Boredom;
  int m_Level;
  int m_LvlPoint;
  int m_IncrementNum;

  int GetMood() const;
  void PassTime(int time = 1);

};

Critter::Critter(int hunger, int boredom, int level , int lvlPoint , int incrementNum): m_Hunger(hunger), m_Boredom(boredom),m_Level(level),m_LvlPoint(lvlPoint),m_IncrementNum(incrementNum)// constructor definition
{}

inline int Critter::GetMood() const 
{ 
  return (m_Hunger + m_Boredom); 
}

void Critter::PassTime(int time)
{
  //レベルが上がると経過する時間が速くなる
  time = m_Level;
  m_Hunger += time;
  m_Boredom += time;
}

void Critter::Talk() {
  
  cout << "I'm a critter and I feel ";

  int mood = GetMood();
  if (mood > 15)
  {
    cout << "mad.\n";
    m_LvlPoint = 0;//レベルダウンのためにm_LvlPointを0にする
  }
  else if (mood > 10)
  {
    cout << "frustrated.\n";
  }
  else if (mood > 5) 
  {
    cout << "okay.\n";
  }
  else 
  {
    cout << "happy.\n";
    m_LvlPoint ++;//happyが表示されるたびにm_LvlPointに1を加算していく
  }

  cout << "I am ";

  if (m_Hunger > 10)
  {
    cout << "extremely";
  }
  else if (m_Hunger > 5) 
  {
    cout << "very";
  }
  else 
  {
    cout << "not";
  }

  cout << " hungry.\n";

  cout << "I am ";

  if (m_Boredom > 10) 
  {
    cout << "extremely";
  } 
  else if (m_Boredom > 5) 
  {
    cout << "very";
  } else {
    cout << "not";
  }
  
  cout << " bored.\n";

  if(m_LvlPoint == 0 && oneTime == false)//クリッターの状態がmadでm_LevelPointが0になると、レベルが下がり下記のアナウンスを１回だけ表示する
  {
    cout <<"Unfortunately, the critter have gone down a level.\n";
    oneTime = true;
  }

  int LevelUp = 5-m_LvlPoint;//あと何ポイントでレベルアップするか計算する
  
  if(LevelUp >0)//LevelUpが0以上であれば下記を表示する
  {
    cout << LevelUp <<" more to level up.\n";
  }
  else if(LevelUp == 0 && oneTime ==true)//LevelUpが0でレベルアップしたのであれば下記を１回だけ表示する
  {
    cout << "Critters have leveled up.\n";
    oneTime = false;
  }
  

  PassTime();
}

void Critter::Eat(int food) 
{
  cout << "Brruppp.\n";
  
  m_Hunger -= food;
  if (m_Hunger < 0) 
  {
    m_Hunger = 0;
  }

  PassTime();

  m_Hunger -= m_IncrementNum;//時間経過後にincrementNumの分だけ減少させる
  if (m_Hunger < 0) 
  {
    m_Hunger = 0;
  }
}

void Critter::Play(int fun) 
{
  cout << "Wheee!\n";

  m_Boredom -= fun;
  if (m_Boredom < 0) 
  {
    m_Boredom = 0;
  }

  PassTime();

  m_Boredom -= m_IncrementNum;//時間経過後にincrementNumの分だけ減少させる
  if (m_Boredom < 0) 
  {
    m_Boredom = 0;
  }
}

void Critter::LevelUpDown()
{
  //m_LvlPointが5以上だとレベルが上がり、未満になるとレベルが下がる処理
  if(m_LvlPoint >= 5)
  {
    m_Level = 2;
  }else
  {
    m_Level = 1;
  }
}

void Critter::GetValues() const 
{
  cout << "Hunger Lvl: " << m_Hunger << endl;
  cout << "Boredom Lvl: " << m_Boredom << endl;
  cout << "CritterLevel Lvl: "<< m_Level << endl;//クリッターのレベルを表示する
  cout << "Supply Capacity: " << m_IncrementNum << endl;//供給能力を表示する
}

void Critter::Medical() 
{ 
  if (count == 0) 
  { 
    m_Hunger = 0; 
    m_Boredom = 0; 
    count = 1; 
    cout << "got the best";
    //medical関数を追加してm_Hungerとm_Boredomの値を０にする。
    //１回しか使えないようにif文を使う。
   }
   else 
   { 
     cout << "I can't go to the hospital anymore"; 
   } 
}

void Critter::SupplyCapaBoost()
{
  if(m_Level == 2)
  {
    m_IncrementNum++;
  }
  else
  {
    cout<<"Let's do it in your own capacity.\n";
  }
  
}


int main() {
  Critter crit;
  crit.Talk();
  int choice;
  
  do 
  {
    cout << "\nCritter Caretaker\n\n";
    cout << "0 - Quit\n";
    cout << "1 - Listen to your critter\n";
    cout << "2 - Feed your critter\n";
    cout << "3 - Play with your critter\n";
    cout << "4 - List values of hunger and boredom\n";
    cout << "5 - Go hospital\n";
    cout << "6 - <help tool> Strengthening the ability to eliminate Hunger and Boredom\n\n";

    crit.LevelUpDown();
    
    cout << "Choice: ";
    cin >> choice;

    switch (choice)
    {
    case 0:
      cout << "Good-bye.\n";
      break;
    case 1:
      crit.Talk();
      break;
    case 2:
      crit.Eat();
      break;
    case 3:
      crit.Play();
      break;
    case 4:
      crit.GetValues();
      break;
    case 5:
      crit.Medical();
      break;
    case 6:
      crit.SupplyCapaBoost();
      break;
    default:
      cout << "\nSorry, but " << choice << " isn't a valid choice.\n";
    }
    
  } while (choice != 0);

  return 0;
}