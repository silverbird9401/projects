# projects

namespace SpartaDungeon
{
    using System;
    using System.Collections.Generic;
    using System.Linq;

    namespace SpartaRPG
    {
        class Program
        {
            static Character player = new Character();
            static Shop shop = new Shop();

            static void Main(string[] args)
            {
                while (true)
                {
                    Console.Clear();
                    Console.WriteLine("스파르타 마을에 오신 여러분 환영합니다.");
                    Console.WriteLine("이곳에서 던전으로 들어가기전 활동을 할 수 있습니다.");

                    Console.WriteLine("1. 상태 보기");
                    Console.WriteLine("2. 인벤토리");
                    Console.WriteLine("3. 상점");
                    Console.Write("원하시는 행동을 입력해주세요.");

                    string input = Console.ReadLine();

                    switch (input)
                    {
                        case "1":
                            ShowStatus();
                            break;
                        case "2":
                            ShowInventory();
                            break;
                        case "3":
                            ShowShop();
                            break;
                        default:
                            Console.WriteLine("잘못된 입력입니다.");
                            Pause();
                            break;
                    }
                }
            }

            static void ShowStatus()
            {
                Console.Clear();
                Console.WriteLine("상태 보기");
                Console.WriteLine("캐릭터의 정보가 표시됩니다.");

                Console.WriteLine($"Lv. {player.Level}");
                Console.WriteLine($"{player.Name} ( {player.Job} )");
                Console.WriteLine($"공격력 : {player.TotalAttack}");
                Console.WriteLine($"방어력 : {player.TotalDefense}");
                Console.WriteLine($"체 력 : {player.HP}");
                Console.WriteLine($"Gold : {player.Gold} ");

                Console.WriteLine("0. 나가기");
                Console.Write("원하시는 행동을 입력해주세요.");
                Console.ReadLine();
            }
