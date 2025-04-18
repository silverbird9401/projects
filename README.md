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
                    Console.WriteLine("이곳에서 던전으로 들어가기전 활동을 할 수 있습니다.\n");

                    Console.WriteLine("1. 상태 보기");
                    Console.WriteLine("2. 인벤토리");
                    Console.WriteLine("3. 상점");
                    Console.Write("\n원하시는 행동을 입력해주세요.\n>> ");

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
                            Console.WriteLine("\n잘못된 입력입니다.");
                            Pause();
                            break;
                    }
                }
            }

            static void ShowStatus()
            {
                Console.Clear();
                Console.WriteLine("상태 보기");
                Console.WriteLine("캐릭터의 정보가 표시됩니다.\n");

                Console.WriteLine($"Lv. {player.Level}");
                Console.WriteLine($"{player.Name} ( {player.Job} )");
                Console.WriteLine($"공격력 : {player.TotalAttack}");
                Console.WriteLine($"방어력 : {player.TotalDefense}");
                Console.WriteLine($"체 력 : {player.HP}");
                Console.WriteLine($"Gold : {player.Gold} G\n");

                Console.WriteLine("0. 나가기");
                Console.Write("원하시는 행동을 입력해주세요.\n>> ");
                Console.ReadLine();
            }

            static void ShowInventory()
            {
                while (true)
                {
                    Console.Clear();
                    Console.WriteLine("인벤토리\n보유 중인 아이템을 관리할 수 있습니다.\n");

                    if (player.Inventory.Count == 0)
                    {
                        Console.WriteLine("[아이템 없음]");
                    }
                    else
                    {
                        Console.WriteLine("[아이템 목록]");
                        foreach (var item in player.Inventory)
                        {
                            Console.WriteLine($"- {(item.Equipped ? "[E]" : "")}{item.Name} | {item.Description}");
                        }
                    }

                    Console.WriteLine("\n1. 장착 관리");
                    Console.WriteLine("0. 나가기");
                    Console.Write("원하시는 행동을 입력해주세요.\n>> ");
                    string input = Console.ReadLine();

                    if (input == "0") break;
                    else if (input == "1") ManageEquip();
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다.");
                        Pause();
                    }
                }
            }

            static void ManageEquip()
            {
                while (true)
                {
                    Console.Clear();
                    Console.WriteLine("인벤토리 - 장착 관리\n보유 중인 아이템을 관리할 수 있습니다.\n");

                    for (int i = 0; i < player.Inventory.Count; i++)
                    {
                        var item = player.Inventory[i];
                        Console.WriteLine($"- {i + 1} {(item.Equipped ? "[E]" : "")}{item.Name} | {item.Description}");
                    }

                    Console.WriteLine("0. 나가기");
                    Console.Write("원하시는 행동을 입력해주세요.\n>> ");
                    string input = Console.ReadLine();
                    if (input == "0") break;

                    if (int.TryParse(input, out int index) && index >= 1 && index <= player.Inventory.Count)
                    {
                        var item = player.Inventory[index - 1];
                        item.Equipped = !item.Equipped;
                        Console.WriteLine(item.Equipped ? "장착 완료" : "장착 해제");
                    }
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다.");
                    }

                    Pause();
                }
            }

            static void ShowShop()
            {
                while (true)
                {
                    Console.Clear();
                    Console.WriteLine("상점\n필요한 아이템을 얻을 수 있는 상점입니다.\n");

                    Console.WriteLine($"[보유 골드] {player.Gold} G\n");
                    Console.WriteLine("[아이템 목록]");
                    for (int i = 0; i < shop.Items.Count; i++)
                    {
                        var item = shop.Items[i];
                        string priceText = item.IsPurchased ? "구매완료" : $"{item.Price} G";
                        Console.WriteLine($"- {i + 1} {item.Name} | {item.Description} |  {priceText}");
                    }

                    Console.WriteLine("\n1. 아이템 구매");
                    Console.WriteLine("0. 나가기");
                    Console.Write("원하시는 행동을 입력해주세요.\n>> ");
                    string input = Console.ReadLine();

                    if (input == "0") break;
                    else if (input == "1") PurchaseItem();
                    else
                    {
                        Console.WriteLine("잘못된 입력입니다.");
                        Pause();
                    }
                }
            }

            static void PurchaseItem()
            {
                Console.Clear();
                Console.WriteLine("상점 - 아이템 구매\n");

                Console.WriteLine($"[보유 골드] {player.Gold} G\n");
                for (int i = 0; i < shop.Items.Count; i++)
                {
                    var item = shop.Items[i];
                    string priceText = item.IsPurchased ? "구매완료" : $"{item.Price} G";
                    Console.WriteLine($"- {i + 1} {item.Name} | {item.Description} |  {priceText}");
                }

                Console.WriteLine("\n0. 나가기");
                Console.Write("원하시는 행동을 입력해주세요.\n>> ");
                string input = Console.ReadLine();

                if (input == "0") return;

                if (int.TryParse(input, out int index) && index >= 1 && index <= shop.Items.Count)
                {
                    var item = shop.Items[index - 1];
                    if (item.IsPurchased)
                    {
                        Console.WriteLine("이미 구매한 아이템입니다.");
                    }
                    else if (player.Gold >= item.Price)
                    {
                        player.Gold -= item.Price;
                        item.IsPurchased = true;
                        player.Inventory.Add(new Item(item)); // 복사본 추가
                        Console.WriteLine("구매를 완료했습니다.");
                    }
                    else
                    {
                        Console.WriteLine("Gold 가 부족합니다.");
                    }
                }
                else
                {
                    Console.WriteLine("잘못된 입력입니다.");
                }

                Pause();
            }

            static void Pause()
            {
                Console.WriteLine("\n엔터를 눌러 계속...");
                Console.ReadLine();
            }
        }

        class Character
        {
            public int Level { get; set; } = 1;
            public string Name { get; set; } = "Chad";
            public string Job { get; set; } = "전사";
            public int HP { get; set; } = 100;
            public int Gold { get; set; } = 1500;
            public List<Item> Inventory { get; set; } = new List<Item>();

            public int TotalAttack => Inventory.Where(i => i.Equipped).Sum(i => i.Attack) + 10;
            public int TotalDefense => Inventory.Where(i => i.Equipped).Sum(i => i.Defense) + 5;
        }

        class Item
        {
            public string Name { get; set; }
            public int Attack { get; set; }
            public int Defense { get; set; }
            public string Description { get; set; }
            public int Price { get; set; }
            public bool IsPurchased { get; set; }
            public bool Equipped { get; set; }

            public Item(string name, int atk, int def, string desc, int price)
            {
                Name = name;
                Attack = atk;
                Defense = def;
                Description = desc;
                Price = price;
            }

            public Item(Item item)
            {
                Name = item.Name;
                Attack = item.Attack;
                Defense = item.Defense;
                Description = item.Description;
                Price = item.Price;
                IsPurchased = true;
            }
        }

        class Shop
        {
            public List<Item> Items { get; set; } = new List<Item>
        {
            new Item("수련자 갑옷", 0, 5, "수련에 도움을 주는 갑옷입니다.", 1000),
            new Item("무쇠갑옷", 0, 9, "무쇠로 만들어져 튼튼한 갑옷입니다.", 1200) { IsPurchased = true },
            new Item("스파르타의 갑옷", 0, 15, "스파르타의 전사들이 사용했다는 전설의 갑옷입니다.", 3500),
            new Item("낡은 검", 2, 0, "쉽게 볼 수 있는 낡은 검 입니다.", 600),
            new Item("청동 도끼", 5, 0, "어디선가 사용됐던거 같은 도끼입니다.", 1500),
            new Item("스파르타의 창", 7, 0, "스파르타의 전사들이 사용했다는 전설의 창입니다.", 1800) { IsPurchased = true },
        };
        }
    }

}
